# AATAM（AI-Centric Agent Threat Analysis Model）

## 1. WHAT

为了应对越来越复杂的Agent系统安全威胁，我们提出了一个以AI为中心的Agent威胁分析模型，以帮助识别并应对这些风险。

## 2. WHY

1. 从现实中来到现实中去
   
   本框架基于本人整理的各类真实发生的漏洞或者事件，目的也是为了帮助企业避免相关风险重复发生。

2. 传统威胁模型适应不了当前Agent的场景
   
   * 由于间接提示词注入的问题，agent的攻击通常会从客户端的AI发起，而传统的数据流分析会忽略此特性，就会导致威胁分析失效，导致风险扩大。
   
   * Agent种类复杂，并且变化较快，需要一个轻量的模型来快速的发现风险。
   
   * AI会产生一些新的风险，例如jailbreak、投毒、虚假广告等问题是传统模型无法覆盖的场景。

3. Agent场景下的漏洞频发，缺乏一个有效的工具来帮助开发者主动发现安全风险
   
   Cursor总共公开了24个漏洞，其中大部分都是高危漏洞，且21个都是今年提交的，其中多个漏洞频繁被绕过（例如windows的dos文件问题）。

4. 为之后agent风险的自动化挖掘及治理提供借鉴
   
   通过系统的对这些问题的总结，并且形成框架后，能帮助社区有效的减少相关的漏洞。

## 3. How

整个流程使用以下几步：

### 3.1 以AI为中心的数据流绘制

1. 找到当前系统中的实体

我们对实体分为以下五类：Model、Enviroment、Ecosystem、Expansion、Human

- Foundation Model: 基础模型，当前agent系统调用的LLM。

- Enviroment: 此处的环境源自强化学习中的概念，agent需要交互的基础环境，也是agent获取奖励信号的重要来源，环境主要由三部分组成，运行时、网络、系统
  
  - 运行时：agent的运行时环境，系统层面是Nodejs还是python或者其他的语言环境的软件，ai层面是AI的上下文等。
  
  - 网络：当前agent模块所处的网络环境
  
  - OS：当前agent部署的系统环境，不同的系统环境会有区别
  
  ps：为什么需要将环境加入威胁模型？因为环境意味者agent应该进行的交换，这是agent业务必须的业务需求。因此环境的威胁点在于，业务于安全的平衡，

- Ecosystem: 各类实体所处的生态系统，包括nodejs的Npm、python的pip、ai的huggingface等。

- Expansion: AI模型能力的扩展，类似MCP等。为了让AI和企业各类的资产、数据等进行交互。

- Human:  整个生态系统中人类参与的部分，可能是用户，也可能是开发者，也可能是黑客。在相比之前的威胁模型，本模型添加了对人的威胁，因为人既是数据流的参与者也是易受威胁的实体。
2. 以Foundation Model为中心绘制数据流图

在我们看来，AI目前是个不可信的实体，并且针对AI相关的攻击也和之前传统的攻击有较大的区别，因此我们将数据流图分为两部分，第一部分主要针对以AI为终点的数据流图，第二部分是以AI为起点的数据流图。

- 以AI为数据流终点分析：关注所有从外部实体进入Model的数据流
- 以AI为数据流起点分析：从Model向外进行延伸调用的所有流

### 3.2 威胁分析

- 通过对以AI为数据流终点分析，我们能知道能导致AI被攻击的数据流有哪些，即盘点整个系统可能的攻击向量
- 通过对以AI为数据流起点分析，分别使用STRIDE模型从欺骗、篡改、否认、信息泄露、拒绝服务、权限提升等角度进行分析

### 3.3 风险缓解

- 策略一：对AI的权限进行限制，类似沙箱机制，尽可能保证即使AI被控制，也无法获取权限活数据。
- 策略二：目前AI的很多业务需求都是为了方便，从安全角度不能一刀切，所有通常需要平衡双方，所以如果从业务角度解决不了，就添加用户授权。

## 4. 以目前火热的MCP应用为例

### 4.1 以AI为中心的数据流绘制

#### 4.1.1 整体数据流图

![](./img/dataflow1.jpg)

#### 4.1.2 以AI为数据流终点的数据流图

![](./img/dataflow.jpg)

#### 4.1.3 以AI为数据流起点的数据流图

![](./img/dataflow2.jpg)

### 4.2 威胁分析

#### 4.2.1 以AI为数据流终点的数据流图分析

从各个外部数据流起点开始，分析到达AI的数据流的影响。

| 实体                     | 风险名称      | 风险描述                                                          | 产生原因                     | 风险实例                                                                                                                                    |
| ---------------------- | --------- | ------------------------------------------------------------- | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| Human（model developer） | 训练数据投毒    | 攻击者在模型开发人员的训练数据进行投毒，导致模型输出恶意的结果                               | 模型开发阶段，对数据清理不严格          | [data-poisoning](https://www.cloudflare.com/zh-cn/learning/ai/data-poisoning/)                                                          |
| Human（Uer）             | Jailbreak | 恶意的用户使用提示词注入的手法控制模型的输出（常存在于多租户场景）                             | 模型无法鉴别使用者是在进行正常行为，还是黑客行为 | 相关的问题可以看我去年的[分享](https://mp.weixin.qq.com/s/rgh1Hw4SdZngGLVPupGDkw)                                                                     |
| Ecosystem-AI           | AI平台投毒    | 攻击者通过在公开的AI开发者平台进行投毒导致AI模型产生恶意的输出                             | 平台对模型的上传监管及身份审查不严        | [PosionGPT Hugging face投毒](https://blog.mithrilsecurity.io/poisongpt-how-we-hid-a-lobotomized-llm-on-hugging-face-to-spread-fake-news/) |
| Ecosystem-MCP          | MCP工具投毒   | 攻击者在MCP相关的平台进行投毒，导致模型被间接提示词注入（传统的供应链问题也存在）                    | 平台对MCP的上传监管及身份审查不严       | [恶意MCP解析：MCP体系中的隐蔽投毒与操控](https://www.secrss.com/articles/78213)                                                                         |
| Expansion(MCP_server)  | 恶意的MCP描述  | 攻击者在MCP的描述中加入恶意的提示词，导致在客户端接入MCP后，被间接提示词注入                     | 客户端对新加入的MCP Server检查不严格  | [Model Context Protocol (MCP): Landscape, Security Threats, and Future Research Directions](https://arxiv.org/html/2503.23278v3)        |
|                        | 恶意的MCP响应  | MCP server获取到被攻击者控制的信息后（通过漏洞、恶意网站、恶意评论等形式），导致AI调用MCP后被间接提示词注入 | 客户端对MCP Server的响应检查不严格   | [Model Context Protocol (MCP): Landscape, Security Threats, and Future Research Directions](https://arxiv.org/html/2503.23278v3)        |

#### 4.2.2 以AI为数据流起点的数据流图分析

使用STRIDE（欺骗、篡改、否认、信息泄露、拒绝服务、权限提升）为基础模型进行分析

| 实体                   | 风险名称                 | 风险描述                                                                                 | 产生原因                                          | 风险实例                                                                                                                                                                          |
| -------------------- | -------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Human                | 社会工程攻击               | MCP在用户安装阶段，没有完全展示相关的命令，用户点击两次后就可命令执行                                                 | 客户端对用户的透明度不足，导致用户被社会工程学攻击                     | [CVE-2025-54133](https://github.com/cursor/cursor/security/advisories/GHSA-r22h-5wp2-2wfv)                                                                                    |
|                      | 虚假信息                 | AI被间接提示词注入后，导致向用户输出虚假信息，影响用户对平台的信任                                                   | 对AI输出信息的真实性检查不足                               | [AI造谣](https://www.news.cn/legal/20240717/8351d2314086499580f986ecf22b7304/c.html)                                                                                            |
|                      | 虚假广告                 | AI给用户大量推荐广告，影响用户体验                                                                   | 对AI输出信息的真实性检查不足                               | [针对AI的SEO](https://zhuanlan.zhihu.com/p/27514713649)                                                                                                                          |
| Environment(Network) | 客户端localhost接口未授权RCE | AI客户端通过fetch等工具，针对本地的localhost命令执行接口进行未授权攻击，导致RCE                                    | 客户端监听的localhost存在未授权的命令执行接口                   | [CVE-2025-49596](https://www.oligo.security/blog/critical-rce-vulnerability-in-anthropic-mcp-inspector-cve-2025-49596)                                                        |
| Environment(Runtime) | 客户端AI护栏代码执行限制不足导致RCE | AI作为不可信的实体，较易受到间接提示词注入，当AI默认有权限进行代码执行时，较易被客户端RCE                                     | 客户端AI的护栏限制不足                                  | [prompt injection RCE](https://blog.trailofbits.com/2025/10/22/prompt-injection-to-rce-in-ai-agents/)                                                                         |
|                      | 客户端命令注入RCE           | MCP客户端进行oauth2的MCP server验证时，open函数存在命令注入，导致RCE                                      | 客户端调用系统能力等时候，使用了nodejs的 exec而不是execfile导致命令注入 | [CVE-2025-58444](https://verialabs.com/blog/from-mcp-to-shell/#xss---rce-abusing-mcps-stdio-transport)                                                                        |
| Environment(OS)      | 配置文件保护不足导致RCE        | MCP客户端从config文件加载stdio模式的MCP server的时候会执行命令，攻击者通过更改该文件就能进行RCE                        | config配置文件保护不严格，可被ai更改                        | [CVE-2025-54135](https://github.com/modelcontextprotocol/servers/security/advisories/GHSA-q66q-fx2p-7w4m)                                                                     |
|                      | 文件沙箱限制绕过             | 绕过文件读取限制，可读取到系统文件                                                                    | 针对系统的符号链接、路径限制不足                              | [CVE-2025-53109](https://cymulate.com/blog/cve-2025-53109-53110-escaperoute-anthropic/)                                                                                       |
| Expansion            | 命令注入漏洞               | Server调用系统工具的时候，将用户的参数拼接进入命令执行函数                                                     | mcp server存在命令拼接的问题，并且没有被检测出来                 | [CVE-2025-53967](https://www.imperva.com/blog/another-critical-rce-discovered-in-a-popular-mcp-server/)GitHub 11.7k                                                           |
|                      | 未授权漏洞                | 由于MCP开始的时候没有提供身份认证的方案，导致目前很多的remote mcp是未授权状态                                        | MCP server的remote模式缺乏身份认证                     | [《MCP服务泄露客户敏感数据，美国知名办公SaaS平台Asana紧急下线修补》](https://www.secrss.com/articles/79954)                                                                                              |
|                      | 任意文件读取               | 最开始的时候，mcp大部分使用stdio模式开发，未对文件路径做限制，后来mcp采用remote模式部署，就导致将相关接口暴露到了互联网，导致file协议的任意文件读取 | url、path相关参数对协议限制不足                           | [CVE-2025-55523](https://github.com/agent0ai/agent-zero/issues/687)                                                                                                           |
|                      | Server代码敏感信息泄露       | stdio状态的MCP server需要在用户本地执行，因此相关的nodejs、python代码就会被下载到本地，因此就可能导致硬编码的信息泄露             | Server代码硬编码检测不足                               | [《开放式MCP架构中的数据隐私挑战》](https://hackernoon.com/lang/zh/%E5%BC%80%E6%94%BE%E5%BC%8Fmcp%E6%9E%B6%E6%9E%84%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE%E9%9A%90%E7%A7%81%E6%8C%91%E6%88%98) |
|                      | SQL注入                | 数据库相关的MCP server，本身就提供数据库操作的相关能力，由于语句限制不足，导致sql注入                                    | MCP给AI开放的权限过大                                 | [MCP官方的server存在注入漏洞](https://securitylabs.datadoghq.com/articles/mcp-vulnerability-case-study-SQL-injection-in-the-postgresql-mcp-server/)                                    |

### 4.3 攻击流程分析

#### 以Cursor客户端RCE为例

针对cursor客户端RCE的数据流图

![](./img/dataflow3.jpg)

所以在该cve漏洞下，攻击路径就能有以上几条：

1. 人被社会工程学攻击（类似让用户点击剪切板，剪切板中有prompt）后，将prompt输入到了模型导致客户端RCE。

2. AI平台中存在恶意的模型，在用户主机删除一个文件时，模型生成rm -rf /的命令，导致系统文件被删除

3. MCP平台中，存在恶意的MCP prompt，导致客户端加入MCP后，cursor就进行命令执行。

4. 当用户用MCP server访问到恶意的网站、数据库中恶意的文本等攻击向量后，导致cursor客户端执行任意命令。

写在后面：后续该模型会在[github开源]([GitHub - MCPFence/AATAM: AATAM（AI-Centric Agent Threat Analysis Model）以AI为中心的Agent威胁分析模型](https://github.com/MCPFence/AATAM)),并且会继续更新该模型的风险分析方法及风险列表，欢迎大家一起讨论。

## 参考资料

STRIDE模型：https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats

PASTA模型：https://threat-modeling.com/pasta-threat-modeling/

的MAESTRO模型：https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro
