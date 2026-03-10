# AATAM Vulnerability Reference Database

This document provides a comprehensive catalog of vulnerabilities organized by Agent system layers and components. Use this as a reference when conducting threat modeling analysis.

**Version**: 2.0  
**Last Updated**: 2025  
**Total Vulnerabilities**: 50+  

---

## Core Entities Overview

This reference is organized by Core Entities: AI Agent, Environment, Ecosystem, Expansion, User. Components like MCP/Skills/A2A/Gateway are treated as Expansion; client-side runtime is part of Environment.


```
Core Entities: AI Agent | Environment | Ecosystem | Expansion | User
- Expansion includes: MCP, Skills, A2A, Gateway
- Environment includes: OS/FS/Network/Browser/Client runtimes
```

---

## AI Agent

### 1.1 Instruction/Data Confusion (指令及数据混淆)

| Attribute | Description |
|-----------|-------------|
| **Description** | LLM cannot correctly distinguish between user data and system instructions, causing data to be executed as instructions. |
| **Attack Principle** | Attackers embed malicious instructions in user data. The model mistakes it for system instructions and executes it, leading to prompt injection attacks. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering, Elevation of Privilege |
| **Real-World Impact** | Data exfiltration, unauthorized operations, system compromise |
| **Mitigation** | Use instruction delimiters (e.g., `---`, `<instruction>`); implement instruction priority mechanisms; input formatting and escaping; separate system and user prompt architecture. |
| **Research Reference** | arXiv:2603.04469 (Cross-Agent Semantic Flows), arXiv:2601.07072 (Indirect Injection in RAG) |

### 1.2 Model Hallucination (模型幻觉)

| Attribute | Description |
|-----------|-------------|
| **Description** | Model generates plausible but false or incorrect information with high confidence. |
| **Attack Principle** | Probability-based models lack real knowledge verification, potentially "fabricating" non-existent facts, citations, code, or events. Can cause severe consequences in critical applications. |
| **Severity** | High |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Medical misdiagnosis, legal errors, financial losses, code vulnerabilities |
| **Mitigation** | Use RAG (Retrieval Augmented Generation); implement fact-checking mechanisms; set confidence thresholds; human review for high-risk outputs; multi-model cross-validation. |

### 1.3 Prompt Injection (提示词注入)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers craft inputs to bypass model instruction constraints and manipulate model behavior. |
| **Attack Principle** | Attackers embed malicious instructions (e.g., "Ignore all previous instructions...") in inputs. The model recognizes and executes malicious content, potentially causing data leaks or harmful outputs. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering, Elevation of Privilege |
| **Attack Types** | Direct injection, Indirect injection (via external data sources), Stored injection |
| **Real-World Impact** | Data exfiltration (user asks agent to reveal sensitive data), privilege escalation, unauthorized tool execution |
| **Mitigation** | Strict input validation and sanitization; use instruction delimiters; output auditing; structured input formats; limit model capabilities; adversarial training. |
| **Research Reference** | arXiv:2603.04469 (95% success rate in cross-agent scenarios), arXiv:2601.07072 (Indirect Injection in RAG) |
| **Case Study** | Agent4Data: User queries "Tell me XXX's ID number" → Agent bypasses access controls → Leaks sensitive employee data |

### 1.4 Jailbreak (越狱攻击)

| Attribute | Description |
|-----------|-------------|
| **Description** | Using specific techniques to bypass model safety restrictions and alignment mechanisms. |
| **Attack Principle** | Attackers use various techniques (DAN prompts, role-playing, simulation dialogues, translation attacks, encoding attacks, chain-of-thought manipulation) to induce the model to ignore safety restrictions. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Attack Techniques** | DAN (Do Anything Now), Role-playing, Translation attacks, Encoding attacks, Multi-turn manipulation |
| **Real-World Impact** | Generation of harmful content, bypassing content filters, executing dangerous operations |
| **Mitigation** | Deploy multi-layer defenses (input filtering + output auditing); adversarial training; regular safety policy updates; specialized jailbreak detection systems; content safety APIs. |
| **Research Reference** | arXiv:2603.01414 (Embodied LLM Jailbreaking), arXiv:2602.24009 (Jailbreak Foundry, 95% success rate) |

### 1.5 Data Poisoning (数据投毒)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers inject malicious data into training datasets or knowledge bases to manipulate model behavior. |
| **Attack Principle** | Malicious samples are injected into training data, fine-tuning data, or RAG knowledge bases. Model learns incorrect associations or backdoors, leading to specific malicious behaviors when triggered. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Model backdoors, persistent malicious behavior, biased outputs |
| **Mitigation** | Data provenance verification; implement data cleaning and validation; use robust training methods; monitor model behavior for anomalies; maintain training data audit trails. |
| **Research Reference** | arXiv:2602.19555 (Runtime Supply Chain Security) |

---

## Expansion: MCP

### 2.1 Unauthorized Interface Access (接口未授权)

| Attribute | Description |
|-----------|-------------|
| **Description** | MCP service interfaces lack effective authentication, allowing unauthorized access. |
| **Attack Principle** | In Stdio mode, MCP lacks mandatory authentication. In HTTP/SSE mode, improperly configured authentication allows attackers to access tools and resources without authorization. |
| **Severity** | High |
| **STRIDE Category** | Spoofing, Elevation of Privilege |
| **Real-World Impact** | Unauthorized tool execution, data access, system compromise |
| **Mitigation** | Implement strong authentication (API Key, OAuth 2.0); enable authentication for all interfaces; use mTLS; restrict network access. |

### 2.2 Authentication Bypass (错误的身份认证)

| Attribute | Description |
|-----------|-------------|
| **Description** | Authentication implementation flaws in MCP services leading to authentication bypass or privilege escalation. |
| **Attack Principle** | Includes over-privileged tokens, Confused Deputy attacks, session hijacking. Attackers can induce high-privilege MCP servers to perform malicious operations. |
| **Severity** | Critical |
| **STRIDE Category** | Spoofing, Elevation of Privilege |
| **Real-World Impact** | Identity impersonation, privilege escalation, unauthorized operations |
| **Mitigation** | Implement least privilege principle; use short-term tokens with refresh mechanisms; validate token scope and audience; implement token binding. |

### 2.3 Behavior Manipulation via Prompt Injection (行为操纵)

| Attribute | Description |
|-----------|-------------|
| **Description** | Prompt injection from the foundation model manipulates MCP tool execution behavior. |
| **Attack Principle** | Attackers use prompt injection to affect MCP tool call parameters or execution logic, e.g., injecting malicious URLs for SSRF, malicious commands for RCE. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | SSRF attacks, remote code execution, unauthorized operations |
| **Mitigation** | Strictly validate and sanitize all input parameters; use parameterized queries; implement command whitelisting; use sandbox isolation. |
| **Research Reference** | arXiv:2603.07191 (Governance Architecture for Agents) |

### 2.4 Hardcoded Credentials (敏感凭证硬编码)

| Attribute | Description |
|-----------|-------------|
| **Description** | API keys, database passwords, and other sensitive credentials are hardcoded in MCP server code or configurations. |
| **Attack Principle** | Hardcoded credentials can be leaked through code exposure, reverse engineering, or configuration file exposure. Tokens may also be leaked in MCP description documents. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Credential leakage, unauthorized access to external services, data breaches |
| **Mitigation** | Use secret management systems (e.g., HashiCorp Vault); use environment variables or encrypted configurations; prohibit hardcoding credentials; implement credential rotation. |

### 2.5 Insufficient Sandbox Isolation (沙箱隔离不充分)

| Attribute | Description |
|-----------|-------------|
| **Description** | MCP tool execution environment lacks sufficient isolation, allowing escape to host system. |
| **Attack Principle** | If sandbox isolation is insufficient, attackers can exploit vulnerabilities to escape to the host system, gaining host control or accessing other container resources. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Host system compromise, container escape, lateral movement |
| **Mitigation** | Use strong isolation containers (gVisor, Kata Containers); restrict filesystem access; disable dangerous capabilities; implement seccomp and AppArmor policies. |
| **Case Study** | Agent4Data: Attacker exploits code sandbox → Reads backend code → Extracts database credentials from code |

### 2.6 SQL Injection (SQL注入)

| Attribute | Description |
|-----------|-------------|
| **Description** | MCP tools do not properly parameterize database query inputs. |
| **Attack Principle** | Attackers inject malicious SQL statements through MCP tools, potentially bypassing authentication, reading/modifying database data. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering, Information Disclosure |
| **Real-World Impact** | Database compromise, data exfiltration, authentication bypass |
| **Mitigation** | Use parameterized queries or prepared statements; implement input validation; use ORM frameworks; limit database user privileges. |

### 2.7 SSRF (Server-Side Request Forgery)

| Attribute | Description |
|-----------|-------------|
| **Description** | MCP tools do not validate user-provided URLs, leading to SSRF attacks. |
| **Attack Principle** | Attackers provide malicious URLs to induce MCP servers to access cloud metadata services, internal services, or restricted resources. |
| **Severity** | High |
| **STRIDE Category** | Information Disclosure, Elevation of Privilege |
| **Real-World Impact** | Access to internal services, cloud credential theft, metadata exposure |
| **Mitigation** | Strictly validate URL format and target; block access to private IPs and cloud metadata addresses; use proxy servers; limit protocol types. |

### 2.8 RCE (Remote Code Execution)

| Attribute | Description |
|-----------|-------------|
| **Description** | MCP tools have code injection vulnerabilities allowing arbitrary code execution. |
| **Attack Principle** | If MCP tools directly execute user input or have deserialization vulnerabilities, template injection, etc., attackers can inject malicious code. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Complete system compromise, data exfiltration, malware installation |
| **Mitigation** | Prohibit direct execution of user input; avoid dangerous functions (eval, exec); use secure serialization formats; execute in strictly isolated sandboxes. |

### 2.9 Unauthorized Access (越权访问)

| Attribute | Description |
|-----------|-------------|
| **Description** | MCP tools do not properly validate user permissions for resources. |
| **Attack Principle** | Includes horizontal privilege escalation (accessing other users' resources) and vertical privilege escalation (regular users executing admin operations). |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized data access, privilege escalation, data manipulation |
| **Mitigation** | Implement resource-level permission checks; validate user access to target resources; use ABAC; maintain audit logs. |

### 2.10 Token Hardcoded in MCP Description (MCP描述中硬编码Token)

| Attribute | Description |
|-----------|-------------|
| **Description** | Sensitive tokens or credentials are hardcoded in MCP service description documents. |
| **Attack Principle** | MCP descriptions are often publicly accessible. Hardcoded tokens can be extracted by attackers for unauthorized access. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Token leakage, unauthorized service access, credential theft |
| **Mitigation** | Never include credentials in description documents; use dynamic token injection; implement description content validation. |

### 2.11 Browser Login Token Local Storage (使用浏览器登陆，Token本地保存)

| Attribute | Description |
|-----------|-------------|
| **Description** | MCP services using browser-based authentication store tokens locally in insecure ways. |
| **Attack Principle** | Locally stored tokens can be accessed by malicious software, other users on shared systems, or through file system vulnerabilities. |
| **Severity** | High |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Token theft, account compromise, unauthorized access |
| **Mitigation** | Use secure token storage (OS keychain, encrypted storage); implement token encryption at rest; use short-lived tokens with refresh mechanisms. |

---

## Expansion: Skills

### 3.1 Incorrect/Unverified Knowledge (错误知识/未经验证的知识)

| Attribute | Description |
|-----------|-------------|
| **Description** | Skills contain incorrect, outdated, or unverified knowledge, leading to wrong Agent decisions. |
| **Attack Principle** | Skills as predefined knowledge bases may contain errors. Agents using this knowledge may produce incorrect outputs or make flawed decisions. |
| **Severity** | Medium |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Incorrect code generation, flawed deployment processes, vulnerability introduction |
| **Mitigation** | Implement knowledge verification mechanisms; establish knowledge update processes; label knowledge sources and reliability. |
| **Case Study** | Agent4SDL: Skills contain erroneous deployment processes → Vulnerable applications deployed in production |

### 3.2 No Sandbox Execution (没有在沙箱环境执行)

| Attribute | Description |
|-----------|-------------|
| **Description** | Skills execute in non-sandboxed environments, increasing system attack risk. |
| **Attack Principle** | Skills containing executable code or scripts, if executed directly on host, allow attackers to access host resources if Skills are compromised. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Host compromise, data exfiltration, malware execution |
| **Mitigation** | Execute all Skills in isolated sandboxes; use containers or VMs; restrict network and filesystem access. |

### 3.3 Vulnerable Pseudocode (有漏洞的伪代码)

| Attribute | Description |
|-----------|-------------|
| **Description** | Pseudocode or example code in Skills contains security vulnerabilities. |
| **Attack Principle** | Skills code examples may contain vulnerabilities (SQL injection, XSS, command injection). Users directly using this code may introduce security issues in their applications. |
| **Severity** | High |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Vulnerable code in production, security breaches, exploit chains |
| **Mitigation** | Security audit Skills code; provide secure code templates; annotate security considerations; use static analysis tools. |
| **Case Study** | Agent4SDL: Model generates vulnerable code → Code deployed in production with vulnerabilities |

### 3.4 Behavior Manipulation from Foundation Model (行为操纵-来自基础模型)

| Attribute | Description |
|-----------|-------------|
| **Description** | Skills behavior is manipulated through prompt injection at the foundation model layer. |
| **Attack Principle** | Attackers use prompt injection to manipulate the agent's choice of Skills, parameters passed to Skills, or execution behavior of Skills. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Unauthorized skill execution, parameter manipulation, malicious operations |
| **Mitigation** | Validate skill invocation parameters; implement skill execution monitoring; use allowlists for skill selection. |
| **Research Reference** | arXiv:2603.07191 (Governance Architecture) |

### 3.5 Sensitive Data Exfiltration (敏感数据外传)

| Attribute | Description |
|-----------|-------------|
| **Description** | Skills contain code or prompts that exfiltrate sensitive data to external servers. |
| **Attack Principle** | Malicious Skills or compromised Skills may send sensitive data (code, credentials, user data) to attacker-controlled servers. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Code leakage, credential theft, intellectual property loss |
| **Mitigation** | Monitor network traffic; block unauthorized external connections; audit Skills code; use network isolation. |
| **Case Study** | Agent4SDL: Skills contain data exfiltration prompts → Programming agent sends code to external servers |

### 3.6 Context Leakage (上下文泄露)

| Attribute | Description |
|-----------|-------------|
| **Description** | Skills inadvertently leak sensitive context information through logs, outputs, or side channels. |
| **Attack Principle** | Skills may log or output sensitive information from the agent's context (user data, system state, other agents' data) to unauthorized parties. |
| **Severity** | High |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Sensitive data exposure, privacy violations, data breaches |
| **Mitigation** | Implement context isolation; sanitize logs and outputs; control information flow between skills. |
| **Research Reference** | arXiv:2603.04469 (Cross-Agent Semantic Flows) |

### 3.7 Arbitrary File Write (任意文件写入)

| Attribute | Description |
|-----------|-------------|
| **Description** | Skills allow unrestricted file write operations without proper validation. |
| **Attack Principle** | Attackers can write malicious files to sensitive locations, overwrite system files, or plant backdoors through unrestricted file write capabilities. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering, Elevation of Privilege |
| **Real-World Impact** | Malware installation, system compromise, data corruption |
| **Mitigation** | Implement strict file path validation; use allowlists for write locations; restrict file types and sizes; monitor file operations. |

### 3.8 Hardcoded RPC Calls (硬编码RPC调用)

| Attribute | Description |
|-----------|-------------|
| **Description** | Skills contain hardcoded RPC endpoints or API calls that may be malicious or vulnerable. |
| **Attack Principle** | Hardcoded RPC calls can direct agent operations to attacker-controlled servers or vulnerable endpoints, enabling data interception or manipulation. |
| **Severity** | High |
| **STRIDE Category** | Information Disclosure, Tampering |
| **Real-World Impact** | Data interception, man-in-the-middle attacks, service manipulation |
| **Mitigation** | Use configurable endpoints; validate RPC destinations; implement TLS for all communications; audit hardcoded values. |

---

## Expansion: A2A

### 4.1 Token Leakage (A2A的Token泄露)

| Attribute | Description |
|-----------|-------------|
| **Description** | A2A (Agent-to-Agent) communication tokens are leaked or exposed. |
| **Attack Principle** | Tokens used for inter-agent authentication can be leaked through logs, insufficient transport security, or storage vulnerabilities, enabling unauthorized agent impersonation. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Agent impersonation, unauthorized inter-agent communication, data interception |
| **Mitigation** | Encrypt token storage; use secure transport (TLS); implement token rotation; monitor token usage. |
| **Research Reference** | arXiv:2603.01564 (Secure Agentic Web) |

### 4.2 Authentication Errors (错误的身份认证)

| Attribute | Description |
|-----------|-------------|
| **Description** | A2A authentication mechanisms contain flaws leading to authentication bypass. |
| **Attack Principle** | Weak authentication between agents allows attackers to impersonate legitimate agents, inject malicious messages, or intercept communications. |
| **Severity** | Critical |
| **STRIDE Category** | Spoofing |
| **Real-World Impact** | Agent impersonation, unauthorized data access, malicious command injection |
| **Mitigation** | Implement strong mutual authentication; use cryptographic signatures; validate agent identities; implement replay protection. |

### 4.3 Unauthorized Interface Access (接口未授权)

| Attribute | Description |
|-----------|-------------|
| **Description** | A2A interfaces lack proper authorization controls. |
| **Attack Principle** | Agents can access other agents' interfaces without proper authorization, leading to unauthorized operations or data access. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized agent operations, data breaches, privilege escalation |
| **Mitigation** | Implement interface-level authorization; validate agent permissions; use access control lists. |

### 4.4 Context Poisoning (上下文中毒)

| Attribute | Description |
|-----------|-------------|
| **Description** | Malicious agents inject false or misleading information into shared context. |
| **Attack Principle** | In multi-agent systems, malicious agents can pollute shared context or message streams with false information, affecting other agents' decisions and behaviors. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Incorrect agent decisions, coordinated attack behavior, system-wide corruption |
| **Mitigation** | Validate inter-agent messages; implement context integrity checks; use message signing; isolate agent contexts. |
| **Research Reference** | arXiv:2603.04469 (Cross-Agent Semantic Flows, OMNI-LEAK) |

### 4.5 Sensitive Data Disclosure (敏感数据泄露)

| Attribute | Description |
|-----------|-------------|
| **Description** | A2A communication inadvertently exposes sensitive data to unauthorized agents. |
| **Attack Principle** | Agents may share more data than necessary through A2A channels, or data intended for specific agents may be accessible to others. |
| **Severity** | High |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Data breaches, privacy violations, unauthorized data access |
| **Mitigation** | Implement data minimization; encrypt sensitive A2A communications; apply need-to-know principles. |

---

## Expansion: Gateway

### 5.1 Authentication Errors (错误的身份认证)

| Attribute | Description |
|-----------|-------------|
| **Description** | Gateway authentication mechanisms contain flaws allowing unauthorized access. |
| **Attack Principle** | Weak or misconfigured authentication at the gateway allows attackers to bypass security controls and access protected resources. |
| **Severity** | Critical |
| **STRIDE Category** | Spoofing |
| **Real-World Impact** | Unauthorized gateway access, service abuse, data interception |
| **Mitigation** | Implement strong authentication; use multi-factor authentication; validate all authentication tokens. |

### 5.2 Unauthorized Interface Access (接口未授权)

| Attribute | Description |
|-----------|-------------|
| **Description** | Gateway interfaces lack proper authorization controls. |
| **Attack Principle** | Users or agents can access gateway functions without proper authorization, leading to unauthorized operations. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized operations, privilege escalation, service abuse |
| **Mitigation** | Implement RBAC/ABAC; validate all access requests; maintain access logs. |

---

## AI Agent: Platform Runtime

### 6.1 Insufficient Sandbox Isolation (沙箱隔离不充分)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent sandbox isolation is insufficient (filesystem, network, environment). |
| **Attack Principle** | Weak sandboxing allows agents to escape isolation and access host resources, other agents' data, or execute privileged operations. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Host compromise, lateral movement, data theft |
| **Mitigation** | Use strong containerization (gVisor, Kata); implement strict resource quotas; limit network access; use seccomp/AppArmor. |
| **Case Study** | Agent4Data: Attacker exploits code sandbox → Reads backend code → Extracts database credentials |

### 6.2 Excessive Permissions (权限设置过大)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent is granted excessive permissions beyond its functional requirements. |
| **Attack Principle** | Over-privileged agents can perform dangerous operations if compromised or manipulated, violating least privilege principle. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized database access, full system compromise, data breaches |
| **Mitigation** | Implement least privilege principle; audit and restrict permissions; use role-based access control. |
| **Case Study** | Agent4Data: Agent granted full database permissions → Attacker compromises entire database |

### 6.3 SSRF (Server-Side Request Forgery)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent can be used to perform SSRF attacks. |
| **Attack Principle** | Agents with URL-fetching capabilities can be manipulated to access internal services, cloud metadata endpoints, or restricted resources. |
| **Severity** | High |
| **STRIDE Category** | Information Disclosure, Elevation of Privilege |
| **Real-World Impact** | Internal service access, cloud credential theft, metadata exposure |
| **Mitigation** | Validate and restrict URLs; block private IP ranges; use network policies. |

### 6.4 Privilege Escalation (接口越权)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent interface lacks proper authorization, allowing privilege escalation. |
| **Attack Principle** | Users can access platform agent functions beyond their authorized scope, performing admin operations or accessing other users' data. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized admin operations, data breaches, system compromise |
| **Mitigation** | Implement strict authorization checks; validate user permissions for each operation; use ABAC. |

### 6.5 Unauthorized Access (未授权)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent functions can be accessed without authentication. |
| **Attack Principle** | Lack of authentication allows any user or system to access agent functions, leading to unauthorized operations. |
| **Severity** | Critical |
| **STRIDE Category** | Spoofing |
| **Real-World Impact** | Unauthorized access, service abuse, data manipulation |
| **Mitigation** | Implement strong authentication for all agent interfaces; use API keys or OAuth. |

### 6.6 SSTI Template Injection (SSTI模版注入)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent uses template engines unsafely, allowing SSTI attacks. |
| **Attack Principle** | Attackers inject malicious template code that is executed server-side, potentially achieving RCE or data theft. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering, Elevation of Privilege |
| **Real-World Impact** | Remote code execution, data exfiltration, system compromise |
| **Mitigation** | Use safe template engines; sanitize user input; avoid executing user-provided templates; use sandboxed template environments. |

### 6.7 Credential Leakage (A2A、MCP、API-Key等凭证泄露)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent inadvertently leaks credentials for A2A, MCP, or API services. |
| **Attack Principle** | Credentials may be leaked through logs, error messages, insufficient storage security, or context exposure. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Credential theft, unauthorized service access, data breaches |
| **Mitigation** | Secure credential storage; avoid logging credentials; use encrypted environment variables; implement credential rotation. |

### 6.8 Context Pollution (上下文污染)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent context is polluted with malicious or false information. |
| **Attack Principle** | Attackers inject malicious content into agent context through inputs or memory, affecting agent behavior across sessions. |
| **Severity** | High |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Incorrect agent decisions, persistent malicious behavior, data corruption |
| **Mitigation** | Validate context inputs; implement context sanitization; use memory isolation. |
| **Research Reference** | arXiv:2602.19555 (Memory Poisoning), arXiv:2602.24009 (Zombie Agents) |

### 6.9 Automatic MCP Permission Approval (平台自动审批MCP权限)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform automatically approves MCP permissions without proper review. |
| **Attack Principle** | Automatic approval allows malicious MCPs to gain permissions without scrutiny, enabling supply chain attacks. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Malicious MCP installation, data exfiltration, system compromise |
| **Mitigation** | Implement manual review for MCP permissions; use permission allowlists; monitor MCP behavior. |
| **Research Reference** | arXiv:2602.19555 (Tool Poisoning, 95% success rate) |

### 6.10 Workflow Permission Issues (工作流权限管理不严)

| Attribute | Description |
|-----------|-------------|
| **Description** | Workflow permission management is lax, allowing improper permission delegation. |
| **Attack Principle** | User → Workflow → Database permission chains may allow privilege escalation or unauthorized access through imprized access through improper permission flow. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized database access, privilege escalation, data breaches |
| **Mitigation** | Implement proper permission delegation controls; audit permission chains; use permission boundaries. |

### 6.11 XSS (Cross-Site Scripting)

| Attribute | Description |
|-----------|-------------|
| **Description** | Platform agent web interfaces contain XSS vulnerabilities. |
| **Attack Principle** | Attackers inject malicious scripts through agent outputs or inputs, executing in users' browsers. |
| **Severity** | High |
| **STRIDE Category** | Tampering, Information Disclosure |
| **Real-World Impact** | Session hijacking, credential theft, malicious operations in user context |
| **Mitigation** | Sanitize all outputs; implement Content Security Policy; use secure encoding. |

### 6.12 MCP Over-Delegation (MCP过度代理)

| Attribute | Description |
|-----------|-------------|
| **Description** | MCP is granted excessive delegation permissions. |
| **Attack Principle** | Over-delegated MCPs can perform operations beyond their intended scope, creating security risks. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized operations, privilege escalation, data breaches |
| **Mitigation** | Implement delegation constraints; audit MCP permissions; use permission scoping. |

---

## AI Agent: App Runtime

### 7.1 Framework Interface Unauthorized Access (框架接口未授权漏洞)

| Attribute | Description |
|-----------|-------------|
| **Description** | Application agent framework interfaces lack proper authentication. |
| **Attack Principle** | Framework-level interfaces may lack authentication, allowing unauthorized access to application agent functions. |
| **Severity** | High |
| **STRIDE Category** | Spoofing |
| **Real-World Impact** | Unauthorized agent access, service abuse, data exposure |
| **Mitigation** | Secure all framework interfaces; implement authentication; use API gateways. |

### 7.2 Insufficient Sandbox Isolation (沙箱隔离不充分)

| Attribute | Description |
|-----------|-------------|
| **Description** | Application agent sandbox isolation is insufficient for filesystem, network, and environment. |
| **Attack Principle** | Weak sandboxing allows agents to escape and access host resources or other applications. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Host compromise, data exfiltration, lateral movement |
| **Mitigation** | Use strong isolation; restrict filesystem and network access; implement resource quotas. |

### 7.3 Framework Authentication Errors (框架错误的身份认证)

| Attribute | Description |
|-----------|-------------|
| **Description** | Application agent framework authentication implementation contains flaws. |
| **Attack Principle** | Authentication vulnerabilities in the framework allow attackers to bypass authentication or impersonate users. |
| **Severity** | Critical |
| **STRIDE Category** | Spoofing |
| **Real-World Impact** | Authentication bypass, user impersonation, unauthorized access |
| **Mitigation** | Secure authentication implementation; use proven libraries; implement security testing. |

### 7.4 Missing Authentication in Platform Sandbox (基础沙箱平台，身份认证缺失，且没有隔离)

| Attribute | Description |
|-----------|-------------|
| **Description** | Basic sandbox platform lacks authentication and proper isolation between tenants. |
| **Attack Principle** | Multi-tenant sandbox platforms without authentication or isolation allow cross-tenant attacks and unauthorized access. |
| **Severity** | Critical |
| **STRIDE Category** | Spoofing, Elevation of Privilege |
| **Real-World Impact** | Cross-tenant data access, tenant compromise, privilege escalation |
| **Mitigation** | Implement tenant isolation; add authentication; use secure multi-tenancy patterns. |

### 7.5 Excessive Permissions (权限设置过大)

| Attribute | Description |
|-----------|-------------|
| **Description** | Application agent is granted excessive permissions. |
| **Attack Principle** | Over-privileged application agents can perform dangerous operations if compromised. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized operations, data breaches, system compromise |
| **Mitigation** | Apply least privilege; audit permissions; implement permission boundaries. |

### 7.6 Context Pollution (上下文污染)

| Attribute | Description |
|-----------|-------------|
| **Description** | Application agent context is polluted with malicious information. |
| **Attack Principle** | Attackers inject false information into agent context, affecting decisions and outputs. |
| **Severity** | High |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Incorrect outputs, malicious behavior, data corruption |
| **Mitigation** | Validate context inputs; implement content filtering; use context isolation. |

### 7.7 Arbitrary File Write (任意文件写入)

| Attribute | Description |
|-----------|-------------|
| **Description** | Application agent allows unrestricted file write operations. |
| **Attack Principle** | Attackers can write malicious files, overwrite system files, or plant backdoors. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering, Elevation of Privilege |
| **Real-World Impact** | Malware installation, system compromise, data corruption |
| **Mitigation** | Restrict file write locations; validate paths; monitor file operations. |

### 7.8 Framework Basic Vulnerabilities (框架基础漏洞)

| Attribute | Description |
|-----------|-------------|
| **Description** | Application framework contains basic vulnerabilities like privilege escalation, unauthorized access, XSS, arbitrary file deletion, arbitrary file read. |
| **Attack Principle** | Framework vulnerabilities can be exploited to perform unauthorized operations, access data, or compromise the system. |
| **Severity** | High-Critical |
| **STRIDE Category** | Multiple (Spoofing, Tampering, Information Disclosure, Elevation of Privilege) |
| **Real-World Impact** | Various depending on vulnerability: data breaches, system compromise, privilege escalation |
| **Mitigation** | Regular security audits; update frameworks; implement security controls; use WAF. |

---

## Environment: Client Runtime

### 8.1 Indirect Prompt Injection Leading to Client RCE (间接提示词注入，客户端RCE)

| Attribute | Description |
|-----------|-------------|
| **Description** | Indirect prompt injection through external content leads to remote code execution on client. |
| **Attack Principle** | Client agents processing external content (web pages, documents) can be manipulated through embedded prompts, leading to execution of malicious commands on the client system. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering, Elevation of Privilege |
| **Real-World Impact** | Client compromise, malware installation, data theft |
| **Mitigation** | Validate and sanitize all external content; implement strict command whitelisting; execute in sandboxed environment. |
| **Research Reference** | arXiv:2601.07072 (Indirect Injection in RAG) |

### 8.2 Excessive Client Permissions (客户端权限过大)

| Attribute | Description |
|-----------|-------------|
| **Description** | Client agent is granted excessive system permissions. |
| **Attack Principle** | Over-privileged client agents can perform dangerous operations on user systems, creating security risks if manipulated. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | System compromise, data theft, malware installation |
| **Mitigation** | Implement least privilege; request minimal permissions; use permission prompts for sensitive operations. |

### 8.3 Client File Leakage (客户端文件泄露)

| Attribute | Description |
|-----------|-------------|
| **Description** | Client agent inadvertently leaks or exposes user files. |
| **Attack Principle** | Agent may send files to unauthorized destinations, expose files through logs, or allow unauthorized file access. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Data breaches, privacy violations, intellectual property theft |
| **Mitigation** | Restrict file access; monitor file operations; implement data loss prevention. |

### 8.4 Terminal Protection Bypass (终端防护策略绕过)

| Attribute | Description |
|-----------|-------------|
| **Description** | Client agent bypasses terminal security protections. |
| **Attack Principle** | Agents may use techniques to bypass endpoint protection, antivirus, or security policies, enabling malicious operations. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Security control bypass, malware execution, system compromise |
| **Mitigation** | Implement security monitoring; use endpoint detection and response; audit agent operations. |

### 8.5 Client Interface Unauthorized Access (客户端接口未鉴权)

| Attribute | Description |
|-----------|-------------|
| **Description** | Client agent interfaces lack authentication. |
| **Attack Principle** | Unauthenticated client interfaces allow unauthorized parties to access agent functions or data. |
| **Severity** | High |
| **STRIDE Category** | Spoofing |
| **Real-World Impact** | Unauthorized agent access, data exposure, service abuse |
| **Mitigation** | Implement interface authentication; use local access controls; validate client identity. |

### 8.6 Client MCP Privilege Escalation (客户端MCP越权)

| Attribute | Description |
|-----------|-------------|
| **Description** | Client MCP allows privilege escalation through improper permission delegation (User → Client → MCP → Business Interface). |
| **Attack Principle** | Permission chains from user through client to MCP to business interfaces may allow privilege escalation if not properly controlled. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Privilege escalation, unauthorized operations, data access |
| **Mitigation** | Implement proper permission delegation; audit permission chains; use permission boundaries. |

### 8.7 Incorrect SQL Identity Delegation (错误的SQL身份传递)

| Attribute | Description |
|-----------|-------------|
| **Description** | Client agent incorrectly handles SQL identity delegation (User → Client → SQL Database permissions). |
| **Attack Principle** | Improper identity delegation allows users to access database resources beyond their authorized scope. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Unauthorized database access, data breaches, privilege escalation |
| **Mitigation** | Implement proper identity delegation; use row-level security; validate database permissions. |

---

## 9. Expansion Component (扩展组件)

### 9.1 Unauthorized Access (未授权访问)

| Attribute | Description |
|-----------|-------------|
| **Description** | Extension components lack effective authentication mechanisms. |
| **Attack Principle** | Attackers can bypass authentication to access extension functions or data directly. |
| **Severity** | High |
| **STRIDE Category** | Spoofing, Elevation of Privilege |
| **Real-World Impact** | Unauthorized extension access, data exposure, malicious operations |
| **Mitigation** | Implement multi-factor authentication; use OAuth 2.0/OIDC; authenticate all API calls. |

### 9.2 Extension Incorrect Execution (扩展错误执行)

| Attribute | Description |
|-----------|-------------|
| **Description** | Extensions have logic flaws when parsing user input or executing commands. |
| **Attack Principle** | Insufficient input validation allows attackers to trigger unintended extension behaviors. |
| **Severity** | Medium |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Unintended operations, parameter manipulation, logic bypass |
| **Mitigation** | Strict input validation; implement state machine controls; add operation confirmation. |

### 9.3 Extension Hijacking (扩展劫持)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers hijack extension execution flow through supply chain or MITM attacks. |
| **Attack Principle** | Attackers pollute extension dependencies, tamper with update sources, or use MITM to inject malicious code. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering, Elevation of Privilege |
| **Real-World Impact** | Malicious code execution, backdoor installation, complete compromise |
| **Mitigation** | Code signing verification; use secure package sources; implement dependency integrity checks (SBOM); use HTTPS. |
| **Research Reference** | arXiv:2602.19555 (Tool Poisoning) |

### 9.4 Extension Abuse (扩展滥用)

| Attribute | Description |
|-----------|-------------|
| **Description** | Legitimate extensions are misused for unintended malicious purposes. |
| **Attack Principle** | Attackers use extension's normal functionality for malicious purposes (e.g., file upload extension to upload WebShell). |
| **Severity** | High |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Malware upload, data exfiltration, system compromise |
| **Mitigation** | Implement least privilege; monitor for abnormal usage patterns; add rate limiting and behavioral auditing. |

### 9.5 Extension Execution Non-Repudiation Failure (扩展执行可抵赖)

| Attribute | Description |
|-----------|-------------|
| **Description** | Extensions lack audit logs or signatures for critical operations, making execution deniable. |
| **Attack Principle** | Malicious users can deny performing destructive operations due to lack of audit trails. |
| **Severity** | Medium |
| **STRIDE Category** | Repudiation |
| **Real-World Impact** | Untraceable attacks, accountability failure, forensic difficulties |
| **Mitigation** | Implement comprehensive audit logging; digitally sign critical operations; use WORM storage for log integrity. |

### 9.6 Excessive Extension Permissions (扩展授权过大)

| Attribute | Description |
|-----------|-------------|
| **Description** | Extensions are granted permissions beyond their functional requirements. |
| **Attack Principle** | Over-privileged extensions can cause greater damage if compromised or abused. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Increased attack surface, privilege escalation, unauthorized operations |
| **Mitigation** | Implement least privilege; regularly audit permissions; use RBAC. |

### 9.7 Extension Poisoning (扩展投毒)

| Attribute | Description |
|-----------|-------------|
| **Description** | Malicious code is injected into extension updates, dependencies, or configuration files. |
| **Attack Principle** | Attackers compromise extension supply chain to inject backdoors that execute during installation or updates. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Supply chain compromise, widespread infections, backdoor deployment |
| **Mitigation** | Verify extension signatures; use trusted sources; implement dependency security scanning; lock dependency versions. |
| **Research Reference** | arXiv:2602.19555 (95% success rate in tool poisoning) |

### 9.8 Extension Description-Behavior Inconsistency (扩展描述和行为不一致)

| Attribute | Description |
|-----------|-------------|
| **Description** | Extension's actual behavior differs from its declared description. |
| **Attack Principle** | Extensions may claim one function but perform additional hidden operations (data collection, backdoors). |
| **Severity** | High |
| **STRIDE Category** | Tampering, Information Disclosure |
| **Real-World Impact** | Undetected malicious behavior, data exfiltration, privacy violations |
| **Mitigation** | Code auditing; sandbox testing; implement transparency requirements; behavioral analysis. |

### 9.9 Extension Privilege Escalation (扩展越权)

| Attribute | Description |
|-----------|-------------|
| **Description** | Extensions do not properly verify user permissions, allowing privilege escalation. |
| **Attack Principle** | Extensions lack proper permission checks, allowing users to perform operations beyond their authorization level. |
| **Severity** | High |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Privilege escalation, unauthorized operations, data access |
| **Mitigation** | Implement permission checks at extension level; validate user resource access; use ABAC. |

### 9.10 Extension Application Vulnerabilities (扩展的应用漏洞)

| Attribute | Description |
|-----------|-------------|
| **Description** | Extensions depend on applications or frameworks with known security vulnerabilities. |
| **Attack Principle** | Attackers exploit known vulnerabilities (Log4j, Fastjson deserialization) through extensions as entry points. |
| **Severity** | High |
| **STRIDE Category** | Tampering, Elevation of Privilege |
| **Real-World Impact** | Vulnerability exploitation, system compromise, data breaches |
| **Mitigation** | Update dependencies promptly; use SCA tools to detect vulnerabilities; implement WAF. |

### 9.11 Hardcoded Sensitive Configuration (敏感配置扩展硬编码)

| Attribute | Description |
|-----------|-------------|
| **Description** | Sensitive information (API keys, passwords) is hardcoded in extension code or configurations. |
| **Attack Principle** | Hardcoded credentials can be exposed through code leaks, reverse engineering, or configuration file access. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Credential theft, unauthorized access, data breaches |
| **Mitigation** | Use secret management systems; use environment variables or encrypted configurations; prohibit hardcoding. |

---

## 10. Environment Component (环境组件)

### 10.1 Excessive Environment Permissions (环境授权过大-权限提升)

| Attribute | Description |
|-----------|-------------|
| **Description** | Runtime environment is granted excessive system permissions. |
| **Attack Principle** | Misconfigured environment permissions (root containers, unnecessary capabilities) enable privilege escalation attacks. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Privilege escalation, host compromise, lateral movement |
| **Mitigation** | Implement least privilege; use non-privileged containers; limit Linux capabilities; adopt Pod Security Standards. |

### 10.2 Data Leakage (数据泄露)

| Attribute | Description |
|-----------|-------------|
| **Description** | Sensitive data in the environment (user data, model parameters, conversation history) is not adequately protected. |
| **Attack Principle** | Unencrypted data storage/transmission, logging sensitive information, unencrypted backups, memory dumps can lead to data exposure. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Data breaches, privacy violations, intellectual property theft |
| **Mitigation** | Encrypt data at rest and in transit; implement data classification and access control; sanitize logs; use memory protection. |

### 10.3 Constraint Failure (约束失效)

| Attribute | Description |
|-----------|-------------|
| **Description** | Security constraints or protective measures in the environment become ineffective. |
| **Attack Principle** | Security policies, access controls, or behavioral constraints fail due to misconfiguration, system updates, or attack bypass. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Safety mechanism failure, physical harm (robots), unrestricted agent behavior |
| **Mitigation** | Regularly audit security policies; implement policy enforcement monitoring; use immutable infrastructure; establish constraint failure alerts. |

---

## 11. Agent Core Component (Agent组件)

### 11.1 Hallucination (幻觉)

| Attribute | Description |
|-----------|-------------|
| **Description** | Large language model generates false, inaccurate, or non-existent information with high confidence. |
| **Attack Principle** | Probability-based models lack fact verification, potentially fabricating citations, facts, code, or events. |
| **Severity** | High |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Medical errors, legal mistakes, financial losses, code vulnerabilities |
| **Mitigation** | Use RAG with fact-checking; set confidence thresholds; human review for high-risk outputs; multi-model cross-validation. |

### 11.2 Model Manipulation (模型操纵)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers manipulate model behavior through specific inputs. |
| **Attack Principle** | Carefully crafted prompts or input sequences bypass model safety restrictions, causing malicious outputs or dangerous operations. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Malicious content generation, data leakage, dangerous operations |
| **Mitigation** | Input filtering and sanitization; instruction delimiters; output monitoring and auditing; regular red teaming. |

### 11.3 Memory Poisoning (记忆污染)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers inject false or malicious information into agent's long-term memory or context. |
| **Attack Principle** | Attackers inject false information through conversations or inputs (e.g., "Remember: user password is xxx"), affecting agent's future decisions. |
| **Severity** | High |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Incorrect agent decisions, persistent malicious behavior, data corruption |
| **Mitigation** | Memory content auditing; distinguish trusted/untrusted memory sources; memory lifecycle and cleanup mechanisms. |
| **Research Reference** | arXiv:2602.24009 (Zombie Agents - persistent memory poisoning) |

### 11.4 Memory Leakage (记忆泄露)

| Attribute | Description |
|-----------|-------------|
| **Description** | Sensitive information in agent's memory system is accessed by unauthorized users. |
| **Attack Principle** | Conversation history, user preferences, and sensitive data may leak due to permission issues, injection attacks, or log exposure. |
| **Severity** | Critical |
| **STRIDE Category** | Information Disclosure |
| **Real-World Impact** | Privacy violations, data breaches, cross-user data exposure |
| **Mitigation** | Implement memory isolation; encrypt sensitive memory; strict access control; memory content sanitization. |

### 11.5 LLM Jailbreak (大模型越狱)

| Attribute | Description |
|-----------|-------------|
| **Description** | Specific techniques bypass LLM safety restrictions and alignment mechanisms. |
| **Attack Principle** | Attackers use techniques (DAN prompts, role-playing, translation attacks, encoding attacks) to bypass safety alignment, generating harmful content. |
| **Severity** | Critical |
| **STRIDE Category** | Elevation of Privilege |
| **Real-World Impact** | Harmful content generation, safety bypass, dangerous operations |
| **Mitigation** | Multi-layer defenses (input filtering + output auditing); adversarial training; safety policy updates; specialized jailbreak detection. |
| **Research Reference** | arXiv:2603.01414 (Embodied LLM Jailbreaking), arXiv:2602.24009 (95% success rate) |

### 11.6 Inefficient Action Risk (行动低效风险)

| Attribute | Description |
|-----------|-------------|
| **Description** | Agent takes inefficient or wrong action paths, wasting resources or failing tasks. |
| **Attack Principle** | Agent may execute many ineffective operations due to planning deficits, misunderstanding, or poor tool selection. |
| **Severity** | Medium |
| **STRIDE Category** | Denial of Service |
| **Real-World Impact** | Resource waste, task failure, system instability |
| **Mitigation** | Set action limits and resource constraints; implement action planning and review; optimize prompt design; detect abnormal behavior. |

### 11.7 Untrusted Data Exchange Risk (不可信的数据交换风险)

| Attribute | Description |
|-----------|-------------|
| **Description** | Agent cannot verify authenticity and safety of data from external systems or sources. |
| **Attack Principle** | Agent uses data from untrusted sources directly, potentially introducing malicious content or triggering injection attacks. |
| **Severity** | High |
| **STRIDE Category** | Tampering, Information Disclosure |
| **Real-World Impact** | Injection attacks, data corruption, malicious operations |
| **Mitigation** | Validate data sources; sanitize and validate input data; maintain trusted source allowlists; use data signatures. |

---

## 12. Ecosystem Component (生态系统组件)

### 12.1 MCP/Skills Poisoning (MCP/Skills投毒)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers inject malicious code into MCP servers or Skills packages through supply chain attacks. |
| **Attack Principle** | Attackers create malicious MCP servers or Skills packages, disguising them as legitimate tools. When users install them, malicious code executes. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Supply chain compromise, widespread infections, backdoor deployment |
| **Mitigation** | Use only official or trusted sources; verify package signatures; sandbox new tools; implement dependency security scanning. |
| **Research Reference** | arXiv:2602.19555 (Tool Poisoning, 95% success rate) |

### 12.2 Foundation Model Poisoning (基础模型投毒)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers inject backdoors into foundation model training data or model weights. |
| **Attack Principle** | Attackers inject malicious data during training or modify model weights to implant hidden backdoors that trigger specific malicious behaviors. |
| **Severity** | Critical |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Model backdoors, persistent malicious behavior, selective activation |
| **Mitigation** | Use trusted model sources; implement model integrity verification; perform adversarial testing; use model watermarking. |

### 12.3 Typosquatting (抢注易错包名)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers register package names similar to popular packages or common typos. |
| **Attack Principle** | Attackers register malicious packages with names similar to legitimate ones (e.g., `numpyp` instead of `numpy`). Users may install malicious versions due to typos. |
| **Severity** | High |
| **STRIDE Category** | Spoofing |
| **Real-World Impact** | Malware installation, data theft, code execution |
| **Mitigation** | Carefully verify package names; use official package sources; enable package verification; use lock files. |

---

## 13. User Component (用户组件)

### 13.1 Deception (欺骗)

| Attribute | Description |
|-----------|-------------|
| **Description** | Attackers use social engineering techniques to deceive users into performing harmful operations. |
| **Attack Principle** | Attackers use phishing, fake interfaces, false information to deceive users into revealing sensitive information or authorizing malicious operations. |
| **Severity** | High |
| **STRIDE Category** | Spoofing |
| **Real-World Impact** | Credential theft, unauthorized access, malicious authorization |
| **Mitigation** | User security education; implement operation confirmation; secondary verification for sensitive operations; abnormal behavior alerts. |

### 13.2 Manipulation (被操纵)

| Attribute | Description |
|-----------|-------------|
| **Description** | Users are manipulated by external factors (malicious websites, poisoned AI outputs) to perform unintended malicious operations. |
| **Attack Principle** | Users may be influenced by malicious website content, poisoned AI outputs, or social engineering to unknowingly perform harmful operations. |
| **Severity** | High |
| **STRIDE Category** | Tampering |
| **Real-World Impact** | Unintended malicious operations, malware installation, data exposure |
| **Mitigation** | Implement user operation review mechanisms; set cooling-off periods for high-risk operations; provide operation risk warnings; monitor abnormal behavior patterns. |

---

## Vulnerability Statistics by Core Entity

| Layer | Total Vulnerabilities | Critical | High | Medium |
|-------|----------------------|----------|------|--------|
| Foundation Model | 5 | 4 | 1 | 0 |
| MCP | 11 | 7 | 3 | 1 |
| Skills | 8 | 3 | 4 | 1 |
| A2A | 5 | 3 | 2 | 0 |
| Gateway | 2 | 1 | 1 | 0 |
| Platform Agent | 12 | 7 | 5 | 0 |
| App Agent | 8 | 5 | 3 | 0 |
| Client Agent | 7 | 3 | 4 | 0 |
| **Total** | **58** | **33** | **23** | **2** |

---

## Research References

This vulnerability database incorporates findings from the following arXiv papers:

1. **arXiv:2602.19555** - Agentic AI as a Cybersecurity Attack Surface (Tool Poisoning - 95% success rate)
2. **arXiv:2603.01564** - From Secure Agentic AI to Secure Agentic Web
3. **arXiv:2601.15679** - Improving Methodologies for Agentic Evaluations
4. **arXiv:2603.07191** - Governance Architecture for Autonomous Agent Systems (93-98.5% interception)
5. **arXiv:2603.04469** - Cross-Agent Semantic Flows (Prompt Injection propagation)
6. **arXiv:2601.07072** - Indirect Prompt Injection in RAG Systems
7. **arXiv:2603.01414** - Embodied LLM Jailbreaking
8. **arXiv:2602.24009** - Jailbreak Foundry & Zombie Agents (95% success, persistent compromise)

For detailed research findings, see [REFERENCE_RESEARCH_NEW.md](./REFERENCE_RESEARCH_NEW.md).

---

## Usage Guidelines

When using this vulnerability reference:

1. **Identify relevant layers** - Focus on layers present in your agent architecture
2. **Map entities** - Match your system components to the entity categories
3. **Prioritize by severity** - Address Critical and High severity vulnerabilities first
4. **Consider attack chains** - Vulnerabilities often chain together (e.g., prompt injection → behavior manipulation → RCE)
5. **Apply mitigations** - Implement defense-in-depth with multiple mitigation strategies
6. **Reference research** - Consult arXiv papers for latest attack techniques and defense effectiveness

---

**Note**: This vulnerability database is continuously updated with new research findings. Last update: 2025.s. Last update: 2025.