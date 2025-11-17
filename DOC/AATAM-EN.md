# AATAM (AI-Centric Agent Threat Analysis Model)

## 1. WHAT

To address the increasingly complex security threats in Agent systems, we propose an AI-centric Agent threat analysis model to help identify and respond to these risks.

## 2. WHY

1. From reality to reality
   
   This framework is based on various real vulnerabilities or incidents that I have compiled, with the purpose of helping enterprises avoid the recurrence of related risks.

2. Traditional threat models cannot adapt to current Agent scenarios
   
   * Due to indirect prompt injection issues, agent attacks often originate from client-side AI, while traditional data flow analysis ignores this characteristic, leading to ineffective threat analysis and expanded risks.
   
   * Agent types are complex and change rapidly, requiring a lightweight model to quickly identify risks.
   
   * AI generates new risks such as jailbreak, poisoning, false advertising, and other issues that traditional models cannot cover.

3. Frequent vulnerabilities in Agent scenarios lack an effective tool to help developers proactively identify security risks
   
   Cursor has publicly disclosed 24 vulnerabilities, most of which are high-risk vulnerabilities, and 21 were submitted this year, with multiple vulnerabilities frequently bypassed (such as Windows DOS file issues).

4. Provide reference for future automated mining and governance of agent risks
   
   Through systematic summary of these issues and framework formation, it can help the community effectively reduce related vulnerabilities.

## 3. How

The entire process uses the following steps:

### 3.1 AI-Centric Data Flow Mapping

1. Identify entities in the current system

We classify entities into the following five categories: Model, Environment, Ecosystem, Expansion, Human

- Foundation Model: The base model, the LLM called by the current agent system.

- Environment: The environment here originates from the concept in reinforcement learning, the environment that the agent needs to interact with, and is also the source of reward signals for the agent. The environment mainly consists of three parts: runtime, network, and system
  
  - Runtime: Software environments such as Node.js, Python, or other language environments
  
  - Network: The network environment where the current agent module is located
  
  - OS: The system environment where the current agent is deployed, different system environments will have differences
  
  PS: Why do we need to include the environment in the threat model? Because the environment means the exchanges that the agent should perform, which is a necessary business requirement for agent operations. Therefore, the threat point of the environment lies in the balance between business and security.

- Ecosystem: The ecosystem where various entities are located, including Node.js's npm, Python's pip, AI's Hugging Face, etc.

- Expansion: Extensions of AI model capabilities, similar to MCP, etc. To enable AI to interact with various enterprise assets, data, etc.

- Human: The part where humans participate in the entire ecosystem, which may be users, developers, or hackers. Compared to previous threat models, this model adds threats to humans, because humans are both participants in data flow and entities vulnerable to threats.

2. Draw data flow diagrams centered on Foundation Model

In our view, AI is currently an untrusted entity, and attacks related to AI are quite different from traditional attacks. Therefore, we divide the data flow diagram into two parts: the first part mainly focuses on data flow diagrams with AI as the endpoint, and the second part is data flow diagrams with AI as the starting point.

- Analysis with AI as data flow endpoint: Focus on all data flows entering the Model from external entities
- Analysis with AI as data flow starting point: All flows extending outward from the Model

### 3.2 Threat Analysis

- Through analysis with AI as the data flow endpoint, we can know which data flows can lead to AI attacks, i.e., inventory all possible attack vectors in the system
- Through analysis with AI as the data flow starting point, use the STRIDE model to analyze from the perspectives of spoofing, tampering, repudiation, information disclosure, denial of service, and elevation of privilege

### 3.3 Risk Mitigation

- Strategy 1: Limit AI permissions, similar to sandbox mechanisms, to ensure that even if AI is controlled, it cannot obtain permissions or data.
- Strategy 2: Currently, many AI business requirements are for convenience. From a security perspective, we cannot make a one-size-fits-all approach, so usually we need to balance both sides. If it cannot be solved from a business perspective, add user authorization.

## 4. Taking the Currently Popular MCP Application as an Example

### 4.1 AI-Centric Data Flow Mapping

#### 4.1.1 Overall Data Flow Diagram

![](./img/dataflow1.jpg)

#### 4.1.2 Data Flow Diagram with AI as Endpoint

![](./img/dataflow.jpg)

#### 4.1.3 Data Flow Diagram with AI as Starting Point

![](./img/dataflow2.jpg)

### 4.2 Threat Analysis

#### 4.2.1 Analysis of Data Flow Diagram with AI as Endpoint

Starting from each external data flow origin, analyze the impact of data flows reaching AI.

| Entity | Risk Name | Risk Description | Root Cause | Risk Example |
| --- | --- | --- | --- | --- |
| Human (model developer) | Training Data Poisoning | Attackers poison the training data of model developers, causing the model to output malicious results | Inadequate data cleaning during model development phase | [data-poisoning](https://www.cloudflare.com/zh-cn/learning/ai/data-poisoning/) |
| Human (User) | Jailbreak | Malicious users use prompt injection techniques to control model output (often exists in multi-tenant scenarios) | Model cannot distinguish whether users are performing normal behavior or hacker behavior | Related issues can be found in my [sharing](https://mp.weixin.qq.com/s/rgh1Hw4SdZngGLVPupGDkw) from last year |
| Ecosystem-AI | AI Platform Poisoning | Attackers poison public AI developer platforms, causing AI models to produce malicious outputs | Inadequate platform supervision of model uploads and identity verification | [PoisonGPT Hugging face poisoning](https://blog.mithrilsecurity.io/poisongpt-how-we-hid-a-lobotomized-llm-on-hugging-face-to-spread-fake-news/) |
| Ecosystem-MCP | MCP Tool Poisoning | Attackers poison MCP-related platforms, causing models to be indirectly prompt injected (traditional supply chain issues also exist) | Inadequate platform supervision of MCP uploads and identity verification | [Malicious MCP Analysis: Hidden Poisoning and Manipulation in MCP Systems](https://www.secrss.com/articles/78213) |
| Expansion(MCP_server) | Malicious MCP Description | Attackers add malicious prompts to MCP descriptions, causing indirect prompt injection when clients connect to MCP | Inadequate client inspection of newly added MCP Servers | [Model Context Protocol (MCP): Landscape, Security Threats, and Future Research Directions](https://arxiv.org/html/2503.23278v3) |
|  | Malicious MCP Response | After MCP server obtains information controlled by attackers (through vulnerabilities, malicious websites, malicious comments, etc.), AI is indirectly prompt injected after calling MCP | Inadequate client inspection of MCP Server responses | [Model Context Protocol (MCP): Landscape, Security Threats, and Future Research Directions](https://arxiv.org/html/2503.23278v3) |

#### 4.2.2 Analysis of Data Flow Diagram with AI as Starting Point

Using STRIDE (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege) as the foundation model for analysis

| Entity | Risk Name | Risk Description | Root Cause | Risk Example |
| --- | --- | --- | --- | --- |
| Human | Social Engineering Attack | During MCP installation, MCP does not fully display related commands, and users can execute commands after clicking twice | Insufficient client transparency to users, leading to users being attacked by social engineering | [CVE-2025-54133](https://github.com/cursor/cursor/security/advisories/GHSA-r22h-5wp2-2wfv) |
|  | False Information | After AI is indirectly prompt injected, it outputs false information to users, affecting user trust in the platform | Insufficient verification of AI output information authenticity | [AI Spreading False Information](https://www.news.cn/legal/20240717/8351d2314086499580f986ecf22b7304/c.html) |
|  | False Advertising | AI recommends a large amount of advertisements to users, affecting user experience | Insufficient verification of AI output information authenticity | [SEO Targeting AI](https://zhuanlan.zhihu.com/p/27514713649) |
| Environment(Network) | Client localhost Interface Unauthorized RCE | AI client uses tools like fetch to conduct unauthorized attacks on local localhost command execution interfaces, leading to RCE | Client-monitored localhost has unauthorized command execution interfaces | [CVE-2025-49596](https://www.oligo.security/blog/critical-rce-vulnerability-in-anthropic-mcp-inspector-cve-2025-49596) |
| Environment(Runtime) | Client AI Guardrail Code Execution Limitation Insufficient Leading to RCE | AI as an untrusted entity is easily subject to indirect prompt injection. When AI has default permissions for code execution, it is easily RCE by the client | Insufficient client AI guardrail limitations | [prompt injection RCE](https://blog.trailofbits.com/2025/10/22/prompt-injection-to-rce-in-ai-agents/) |
|  | Client Command Injection RCE | When MCP client performs oauth2 MCP server verification, the open function has command injection, leading to RCE | Client uses Node.js exec instead of execfile when calling system capabilities, causing command injection | [CVE-2025-58444](https://verialabs.com/blog/from-mcp-to-shell/#xss---rce-abusing-mcps-stdio-transport) |
| Environment(OS) | Configuration File Protection Insufficient Leading to RCE | When MCP client loads stdio mode MCP server from config file, it executes commands. Attackers can perform RCE by changing this file | Config configuration file protection is not strict and can be changed by AI | [CVE-2025-54135](https://github.com/modelcontextprotocol/servers/security/advisories/GHSA-q66q-fx2p-7w4m) |
|  | File Sandbox Limitation Bypass | Bypass file reading limitations to read system files | Insufficient symbolic link and path limitations for the system | [CVE-2025-53109](https://cymulate.com/blog/cve-2025-53109-53110-escaperoute-anthropic/) |
| Expansion | Command Injection Vulnerability | When Server calls system tools, it concatenates user parameters into command execution functions | MCP server has command concatenation issues and was not detected | [CVE-2025-53967](https://www.imperva.com/blog/another-critical-rce-discovered-in-a-popular-mcp-server/) GitHub 11.7k |
|  | Unauthorized Vulnerability | Since MCP did not provide an authentication scheme at the beginning, many remote MCPs are currently in an unauthorized state | Remote mode of MCP server lacks authentication | [MCP Services Leak Customer Sensitive Data, US Well-known Office SaaS Platform Asana Urgently Offline for Repair](https://www.secrss.com/articles/79954) |
|  | Arbitrary File Read | Initially, most MCPs were developed using stdio mode without file path restrictions. Later, MCP adopted remote mode deployment, which exposed related interfaces to the internet, leading to arbitrary file reading through file protocol | Insufficient protocol restrictions on URL and path related parameters | [CVE-2025-55523](https://github.com/agent0ai/agent-zero/issues/687) |
|  | Server Code Sensitive Information Disclosure | MCP servers in stdio state need to be executed locally, so related Node.js and Python code will be downloaded locally, which may lead to hardcoded information disclosure | Insufficient hardcoded information detection in Server code | [Data Privacy Challenges in Open MCP Architecture](https://hackernoon.com/lang/zh/%E5%BC%80%E6%94%BE%E5%BC%8Fmcp%E6%9E%B6%E6%9E%84%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE%E9%9A%90%E7%A7%81%E6%8C%91%E6%88%98) |
|  | SQL Injection | Database-related MCP servers provide database operation capabilities. Due to insufficient statement restrictions, SQL injection occurs | MCP gives AI excessive permissions | [SQL injection vulnerability in official MCP server](https://securitylabs.datadoghq.com/articles/mcp-vulnerability-case-study-SQL-injection-in-the-postgresql-mcp-server/) |

### 4.3 Attack Flow Analysis

#### Taking Cursor Client RCE as an Example

Data flow diagram for Cursor client RCE

![](./img/dataflow3.jpg)

So under this CVE vulnerability, the attack paths can include the following:

1. Humans are attacked by social engineering (such as making users click on clipboard, with prompts in the clipboard), then input prompts to the model causing client RCE.

2. Malicious models exist in AI platforms. When users delete a file on their host, the model generates rm -rf / command, causing system files to be deleted.

3. In MCP platforms, malicious MCP prompts exist, causing Cursor to execute commands after the client joins MCP.

4. When users access malicious websites, malicious text in databases and other attack vectors through MCP server, it causes Cursor client to execute arbitrary commands.

Written at the end: This model will be open sourced on [GitHub](https://github.com/MCPFence/AATAM) in the future, and will continue to update the risk analysis methods and risk lists of this model. Everyone is welcome to discuss together.

## References

STRIDE Model: https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats

PASTA Model: https://threat-modeling.com/pasta-threat-modeling/

MAESTRO Model: https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro