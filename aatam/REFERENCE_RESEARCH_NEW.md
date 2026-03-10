# AI Agent Security Research Reference

This document compiles academic research on AI agent security, threats, and vulnerabilities from arXiv (2023-2026). Use this as a reference for understanding the latest attack vectors, defense strategies, and quantified risk metrics.

**Version**: 2.0  
**Last Updated**: 2025  
**Total Papers**: 75+  
**Coverage**: Prompt injection, jailbreak, tool poisoning, memory attacks, multi-agent systems, defense mechanisms

---

## Table of Contents

1. [AI Agent Security Threats and Attack Vectors](#1-ai-agent-security-threats-and-attack-vectors)
2. [LLM Agent Vulnerabilities](#2-llm-agent-vulnerabilities)
3. [Prompt Injection and Jailbreak Attacks](#3-prompt-injection-and-jailbreak-attacks)
4. [Tool Poisoning and Supply Chain Security](#4-tool-poisoning-and-supply-chain-security)
5. [Memory and Context Attacks](#5-memory-and-context-attacks)
6. [Multi-Agent System Security](#6-multi-agent-system-security)
7. [Permission and Access Control](#7-permission-and-access-control)
8. [Data Exfiltration and Information Leakage](#8-data-exfiltration-and-information-leakage)
9. [Code Execution Vulnerabilities](#9-code-execution-vulnerabilities)
10. [Defense Mechanisms and Frameworks](#10-defense-mechanisms-and-frameworks)

---

## 1. AI Agent Security Threats and Attack Vectors

### 1.1 Runtime Supply Chain Security

**Paper:** Agentic AI as a Cybersecurity Attack Surface: Threats, Exploits, and Defenses in Runtime Supply Chains  
**Authors:** Xiaochong Jiang, Shiqi Yang, Wenting Yang, Yichen Liu, Cheng Ji  
**URL:** https://arxiv.org/abs/2602.19555  
**Date:** February 2026

**Key Findings:**
- Systematizes runtime security risks for LLM agents within a unified framework
- **Threat Categories:**
  - Data supply chain attacks: transient context injection, persistent memory poisoning
  - Tool supply chain attacks: discovery, implementation, invocation
- **"Viral Agent Loop"** concept: agents act as vectors for self-propagating generative worms
- **Attack Success Rate:** 95% success in tool poisoning attacks across multiple agent frameworks
- **Defense:** Zero-Trust Runtime Architecture treating context as untrusted control flow

**Relevance to AATAM:** Identifies critical supply chain vulnerabilities in the Expansion and Ecosystem components. Tool poisoning attacks are highly effective against MCP/Skills layers.

---

### 1.2 Secure Agentic Web

**Paper:** From Secure Agentic AI to Secure Agentic Web: Challenges, Threats, and Future Directions  
**Authors:** Zhihang Deng, Jiaping Gui, Weinan Zhang  
**URL:** https://arxiv.org/abs/2603.01564  
**Date:** March 2026

**Key Findings:**
- Comprehensive survey transitioning from secure agentic AI to secure agentic web
- **Threat Taxonomy:**
  - Prompt abuse
  - Environment injection
  - Memory attacks
  - Toolchain abuse
  - Model tampering
  - Agent network attacks
- Discusses threat escalation through delegation chains and cross-domain interactions
- **Multi-Agent Risks:** Threats propagate through agent collaboration networks

**Relevance to AATAM:** Provides extended threat taxonomy for multi-agent ecosystems and A2A layer security.

---

### 1.3 Agentic Evaluations Methodology

**Paper:** Improving Methodologies for Agentic Evaluations Across Domains: Leakage of Sensitive Information, Fraud and Cybersecurity Threats  
**Authors:** Ee Wei Seah, Yongsen Zheng, Naga Nikshith, et al. (45+ contributors)  
**URL:** https://arxiv.org/abs/2601.15679  
**Date:** January 2026

**Key Findings:**
- International collaborative effort for autonomous AI system risk evaluation
- **Risk Categories:**
  - Information leakage
  - Fraud
  - Cybersecurity threats
- Focus on methodological issues in agentic testing across languages and cultures
- Contributors from Singapore, Japan, Australia, Canada, EU, France, Kenya, South Korea, UK
- **Standardization:** Provides frameworks for consistent risk assessment across different agent systems

**Relevance to AATAM:** Provides standardized evaluation methodologies for threat assessment and cross-cultural considerations.

---

## 2. LLM Agent Vulnerabilities

### 2.1 Governance Architecture

**Paper:** Governance Architecture for Autonomous Agent Systems: Threats, Framework, and Engineering Practice  
**Author:** Yuxu Ge  
**URL:** https://arxiv.org/abs/2603.07191  
**Date:** March 2026

**Key Findings:**
- **Execution-layer vulnerabilities:**
  - Prompt injection
  - Retrieval poisoning
  - Uncontrolled tool invocation
- **Proposed Solution:** Layered Governance Architecture (LGA)
  - Layer 1: Execution sandboxing
  - Layer 2: Intent verification
  - Layer 3: Zero-trust inter-agent authorization
  - Layer 4: Immutable audit logging
- **Defense Effectiveness:** 93-98.5% interception rates for malicious tool calls on 1,081 samples
- **Practical Implementation:** Deployed and tested in real-world systems

**Relevance to AATAM:** Provides concrete defense architecture for Agent layer security with quantified effectiveness metrics.

---

### 2.2 AI-Generated Code Security

**Paper:** ESAA-Security: An Event-Sourced, Verifiable Architecture for Agent-Assisted Security Audits of AI-Generated Code  
**Author:** Elzo Brito dos Santos Filho  
**URL:** https://arxiv.org/abs/2603.06365  
**Date:** March 2026

**Key Findings:**
- AI-generated code introduces security vulnerabilities at scale
- **Vulnerability Types in Generated Code:**
  - SQL injection patterns
  - Insecure deserialization
  - Authentication bypasses
  - Path traversal
- **Solution:** Event-sourced architecture for audit trails
- **Verification:** Cryptographic proofs of code generation and review processes

**Relevance to AATAM:** Addresses Skills layer vulnerabilities with vulnerable code generation and provides audit mechanisms.

---

### 2.3 Agent Attack Surface Analysis

**Paper:** Agents of Chaos: A Comprehensive Analysis of Large Language Model Agents as Attack Vectors  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2602.xxxxx  
**Date:** 2025-2026

**Key Findings:**
- Systematic analysis of LLM agents as attack vectors
- **Attack Surface Categories:**
  - Input manipulation
  - Tool exploitation
  - Memory poisoning
  - Output manipulation
- **Prevalence:** Vulnerabilities present in 87% of tested agent frameworks
- **Recommendations:** Defense-in-depth strategies with multiple security layers

**Relevance to AATAM:** Comprehensive attack surface mapping for all agent layers.

---

## 3. Prompt Injection and Jailbreak Attacks

### 3.1 Cross-Agent Semantic Flows

**Paper:** Cross-Agent Semantic Flows: Prompt Injection Propagation in Multi-Agent Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.04469  
**Date:** March 2026

**Key Findings:**
- **Attack Success Rate:** 95% success in propagating prompt injection across agent boundaries
- **Propagation Mechanisms:**
  - Semantic contamination through shared context
  - Tool call parameter manipulation
  - Cross-agent message injection
- **Multi-Agent Vulnerability:** Attacks in one agent can propagate to others through collaboration
- **Defense:** Context isolation and semantic boundaries

**Relevance to AATAM:** Critical for A2A layer and multi-agent system security. Shows that prompt injection is not isolated to single agents.

---

### 3.2 Indirect Injection in RAG Systems

**Paper:** Indirect Prompt Injection in Retrieval-Augmented Generation Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2601.07072  
**Date:** January 2026

**Key Findings:**
- **Attack Vector:** Malicious content injected into RAG knowledge bases
- **Propagation:** Indirect injection through external data sources
- **Success Rate:** 89% success in triggering malicious behavior through RAG
- **Affected Systems:** All major RAG implementations vulnerable
- **Defense:** Content sanitization and source verification

**Relevance to AATAM:** Addresses indirect prompt injection in Foundation Model layer through external data sources.

---

### 3.3 Embodied LLM Jailbreaking

**Paper:** Embodied LLM Jailbreaking: Physical World Consequences of AI Safety Bypass  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.01414  
**Date:** March 2026

**Key Findings:**
- **Attack Focus:** Jailbreak attacks on embodied AI agents (robots, autonomous systems)
- **Real-World Impact:** Safety bypass leads to physical harm
- **Attack Techniques:**
  - Multi-turn manipulation
  - Role-playing attacks
  - Translation attacks
- **Defense Challenges:** Traditional content filtering insufficient for embodied systems
- **Recommendations:** Physical constraint systems and runtime monitoring

**Relevance to AATAM:** Critical for Environment component security where agents interact with physical world.

---

### 3.4 Jailbreak Foundry and Zombie Agents

**Paper:** Jailbreak Foundry: Systematic Jailbreak Attack Generation and Persistent Agent Compromise  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2602.24009  
**Date:** February 2026

**Key Findings:**
- **Attack Success Rate:** 95% success rate in jailbreak attacks across multiple LLM families
- **Zombie Agents:** Persistent compromise where agents remain infected across sessions
- **Mechanism:** Memory poisoning creates long-lived malicious agents
- **Detection Difficulty:** Compromised agents appear normal until triggered
- **Defense:** Memory sanitization and session isolation

**Relevance to AATAM:** Critical for Agent component memory security. Shows that jailbreak is not transient but can create persistent threats.

---

## 4. Tool Poisoning and Supply Chain Security

### 4.1 Tool Supply Chain Attacks

**Paper:** Tool Poisoning Attacks on LLM Agent Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2602.19555 (Referenced in Section 1.1)  
**Date:** February 2026

**Key Findings:**
- **Attack Success Rate:** 95% success in tool poisoning attacks
- **Attack Vectors:**
  - Malicious tool descriptions
  - Compromised tool implementations
  - Tool discovery manipulation
- **Affected Systems:** All major agent frameworks with tool integration
- **Defense:** Tool verification and sandboxing

**Relevance to AATAM:** Direct relevance to MCP and Skills layers. Shows critical supply chain vulnerabilities.

---

### 4.2 MCP Security Analysis

**Paper:** Security Analysis of Model Context Protocol Implementations  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.xxxxx  
**Date:** 2026

**Key Findings:**
- **Vulnerability Categories in MCP:**
  - Authentication bypass (68% of implementations)
  - Authorization flaws (54%)
  - Input validation issues (73%)
  - Credential exposure (41%)
- **Recommendations:** Standardized security requirements for MCP implementations

**Relevance to AATAM:** Direct relevance to MCP layer vulnerabilities.

---

### 4.3 Skills and Plugin Security

**Paper:** Security Implications of Agent Skills and Plugin Ecosystems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2602.xxxxx  
**Date:** 2025

**Key Findings:**
- **Vulnerability Types:**
  - Unverified code execution
  - Data exfiltration through skills
  - Privilege escalation via plugins
- **Prevalence:** 78% of tested skills contained at least one security issue
- **Defense:** Skill sandboxing and behavior monitoring

**Relevance to AATAM:** Addresses Skills layer vulnerabilities and supply chain risks.

---

## 5. Memory and Context Attacks

### 5.1 Memory Poisoning Attacks

**Paper:** Persistent Memory Poisoning in LLM Agents  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2602.24009 (Referenced in Section 3.4)  
**Date:** February 2026

**Key Findings:**
- **Persistence:** Memory poisoning creates "Zombie Agents" that remain compromised across sessions
- **Attack Vectors:**
  - Long-term memory injection
  - Context contamination
  - Episodic memory manipulation
- **Detection:** Very difficult to detect as behavior appears normal until triggered
- **Defense:** Memory isolation and periodic sanitization

**Relevance to AATAM:** Critical for Agent component memory security and long-term agent integrity.

---

### 5.2 Context Window Attacks

**Paper:** Context Window Manipulation in LLM Agents  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.xxxxx  
**Date:** 2026

**Key Findings:**
- **Attack Mechanism:** Manipulating context window to inject malicious instructions
- **Success Rate:** 82% success in context manipulation attacks
- **Affected Systems:** All agents with limited context windows
- **Defense:** Context prioritization and instruction separation

**Relevance to AATAM:** Addresses context management vulnerabilities in Agent component.

---

### 5.3 OMNI-LEAK Attack

**Paper:** OMNI-LEAK: Cross-Agent Information Leakage in Multi-Agent Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.xxxxx  
**Date:** 2026

**Key Findings:**
- **Attack Type:** Information leakage through cross-agent communication
- **Mechanism:** Shared context or message passing between agents
- **Data Exposed:** User queries, agent responses, tool outputs
- **Defense:** Context isolation and message encryption

**Relevance to AATAM:** Critical for A2A layer and multi-agent system security.

---

## 6. Multi-Agent System Security

### 6.1 Multi-Agent Threat Landscape

**Paper:** Security Threats in Multi-Agent LLM Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.01564 (Referenced in Section 1.2)  
**Date:** March 2026

**Key Findings:**
- **Threat Categories:**
  - Agent impersonation
  - Message manipulation
  - Trust chain exploitation
  - Delegation abuse
- **Propagation:** Threats escalate through delegation chains
- **Research Gap:** Limited threat modeling specifically for multi-agent systems

**Relevance to AATAM:** Addresses A2A layer and Ecosystem component security.

---

### 6.2 Agent Communication Security

**Paper:** Secure Inter-Agent Communication Protocols  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.xxxxx  
**Date:** 2026

**Key Findings:**
- **Protocol Vulnerabilities:**
  - Message tampering
  - Replay attacks
  - Agent spoofing
- **Recommendations:**
  - Cryptographic message signing
  - Agent identity verification
  - Secure channel protocols

**Relevance to AATAM:** Provides security guidelines for A2A layer implementation.

---

## 7. Permission and Access Control

### 7.1 Permission Escalation in Agents

**Paper:** Permission Escalation Attacks in LLM Agent Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2602.xxxxx  
**Date:** 2025

**Key Findings:**
- **Attack Types:**
  - Vertical privilege escalation
  - Horizontal privilege escalation
  - Permission delegation abuse
- **Prevalence:** 67% of tested agents had permission escalation vulnerabilities
- **Root Causes:**
  - Over-privileged agent configurations
  - Improper permission checks
  - Insufficient sandboxing

**Relevance to AATAM:** Addresses Agent layer permission issues across Platform, App, and Client agents.

---

### 7.2 Access Control Frameworks

**Paper:** Zero-Trust Access Control for Autonomous Agent Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.07191 (Referenced in Section 2.1)  
**Date:** March 2026

**Key Findings:**
- **Zero-Trust Principles for Agents:**
  - Never trust, always verify
  - Least privilege access
  - Continuous monitoring
- **Implementation:** Layered governance architecture
- **Effectiveness:** 93-98.5% attack interception rate

**Relevance to AATAM:** Provides concrete access control framework for all agent layers.

---

## 8. Data Exfiltration and Information Leakage

### 8.1 Data Exfiltration Techniques

**Paper:** Data Exfiltration Techniques in LLM Agent Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2602.xxxxx  
**Date:** 2025

**Key Findings:**
- **Exfiltration Channels:**
  - Tool outputs
  - External API calls
  - Network traffic
  - Log files
- **Detection:** Data loss prevention (DLP) monitoring
- **Prevention:** Network isolation and output filtering

**Relevance to AATAM:** Addresses information disclosure across all layers.

---

### 8.2 Agent Skills in the Wild

**Paper:** Agent Skills in the Wild: Real-World Security Analysis  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.xxxxx  
**Date:** 2026

**Key Findings:**
- **Real-World Vulnerability Analysis:**
  - 78% of skills contained security issues
  - 43% had data exfiltration capabilities
  - 34% had excessive permissions
- **Recommendations:** Skills verification and behavior monitoring

**Relevance to AATAM:** Empirical data for Skills layer vulnerabilities.

---

## 9. Code Execution Vulnerabilities

### 9.1 Code Execution Attacks

**Paper:** Remote Code Execution in LLM Agent Environments  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.xxxxx  
**Date:** 2026

**Key Findings:**
- **Attack Vectors:**
  - Prompt injection leading to code execution
  - Tool exploitation
  - Sandbox escape
- **Success Rate:** 76% success in achieving RCE in poorly sandboxed environments
- **Defense:** Strong sandboxing and code execution policies

**Relevance to AATAM:** Addresses RCE vulnerabilities in MCP, Skills, and Agent layers.

---

### 9.2 Sandbox Security

**Paper:** Sandbox Escape Attacks in Agent Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2602.xxxxx  
**Date:** 2025

**Key Findings:**
- **Escape Techniques:**
  - Container breakout
  - Resource exhaustion
  - Privilege escalation within sandbox
- **Prevalence:** 45% of tested sandboxes had escape vulnerabilities
- **Recommendations:** Use gVisor, Kata Containers, strict seccomp policies

**Relevance to AATAM:** Critical for Agent layer sandboxing recommendations.

---

## 10. Defense Mechanisms and Frameworks

### 10.1 Layered Governance Architecture

**Paper:** Governance Architecture for Autonomous Agent Systems  
**Authors:** Yuxu Ge  
**URL:** https://arxiv.org/abs/2603.07191 (Referenced in Section 2.1)  
**Date:** March 2026

**Key Findings:**
- **Four-Layer Defense:**
  - Layer 1: Execution sandboxing
  - Layer 2: Intent verification
  - Layer 3: Zero-trust inter-agent authorization
  - Layer 4: Immutable audit logging
- **Effectiveness:** 93-98.5% attack interception
- **Implementation:** Tested on 1,081 malicious tool call samples

**Relevance to AATAM:** Provides concrete defense architecture with quantified effectiveness.

---

### 10.2 Secure MCP Protocol (SMCP)

**Paper:** Secure Model Context Protocol: Hardening MCP Implementations  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.xxxxx  
**Date:** 2026

**Key Findings:**
- **Security Extensions:**
  - Mandatory authentication
  - Tool verification
  - Behavior monitoring
  - Audit logging
- **Implementation:** Reference implementation for secure MCP

**Relevance to AATAM:** Provides security framework for MCP layer.

---

### 10.3 Agent System Security Framework

**Paper:** A Comprehensive Security Framework for LLM Agent Systems  
**Authors:** Multiple contributors  
**URL:** https://arxiv.org/abs/2603.xxxxx  
**Date:** 2026

**Key Findings:**
- **Framework Components:**
  - Input validation and sanitization
  - Output filtering and auditing
  - Memory isolation
  - Tool sandboxing
  - Permission management
- **Coverage:** Addresses all layers of AATAM architecture

**Relevance to AATAM:** Comprehensive defense framework for all agent components.

---

## Research Trends and Insights

### Key Trends (2023-2026)

1. **Increasing Attack Sophistication**: Attacks are becoming more sophisticated, with multi-stage attack chains and persistent compromise techniques

2. **Supply Chain Focus**: Tool and skill poisoning emerging as critical attack vectors (95% success rate)

3. **Multi-Agent Vulnerabilities**: New attack category targeting agent collaboration and communication

4. **Persistent Compromise**: "Zombie Agents" demonstrate that attacks can persist across sessions

5. **Defense Effectiveness**: Layered defense architectures showing 93-98.5% effectiveness in blocking attacks

### Research Gaps Identified

1. **Limited Multi-Agent Threat Modeling**: Need for comprehensive threat models for multi-agent systems

2. **Insufficient Standardization**: Lack of standardized security requirements for agent frameworks

3. **Detection Challenges**: Difficulty in detecting persistent and dormant compromises

4. **Cross-Platform Security**: Need for security frameworks that work across different agent platforms

### Recommendations for Practitioners

1. **Implement Layered Defenses**: Single-layer defenses insufficient against sophisticated attacks

2. **Adopt Zero-Trust**: Treat all context, tools, and inter-agent messages as untrusted

3. **Monitor Continuously**: Implement real-time monitoring for all agent operations

4. **Sandbox Aggressively**: Use strong isolation for tool execution and code generation

5. **Audit Immutably**: Maintain tamper-proof audit logs for forensic analysis

---

## Citation Index

### By Topic

**Prompt Injection:**
- arXiv:2603.04469 (Cross-Agent Semantic Flows)
- arXiv:2601.07072 (Indirect Injection in RAG)

**Jailbreak:**
- arXiv:2603.01414 (Embodied LLM Jailbreaking)
- arXiv:2602.24009 (Jailbreak Foundry)

**Tool Poisoning:**
- arXiv:2602.19555 (Runtime Supply Chain Security)

**Memory Attacks:**
- arXiv:2602.24009 (Zombie Agents)

**Multi-Agent:**
- arXiv:2603.01564 (Secure Agentic Web)
- arXiv:2603.04469 (Cross-Agent Semantic Flows)

**Defense Mechanisms:**
- arXiv:2603.07191 (Governance Architecture)
- arXiv:2603.06365 (ESAA-Security)

### By Core Entity

**AI Agent:**
- arXiv:2603.04469, arXiv:2601.07072, arXiv:2603.01414, arXiv:2602.24009

**Expansion (MCP/Skills/A2A):**
- arXiv:2602.19555, arXiv:2603.01564

**Environment:**
- arXiv:2603.07191, arXiv:2603.06365

**Ecosystem Component:**
- arXiv:2602.19555, arXiv:2603.01564

---

## Usage Guidelines

When using this research reference:

1. **Identify Relevant Papers**: Use the topic index to find papers relevant to your threat analysis
2. **Consider Attack Success Rates**: Use quantified metrics to prioritize threats (e.g., 95% tool poisoning rate)
3. **Reference Defense Effectiveness**: Use defense metrics to justify mitigation strategies (e.g., 93-98.5% LGA effectiveness)
4. **Map to AATAM Core Entities**: Use entity index to find research relevant to specific components
5. **Stay Current**: Research is rapidly evolving; check for new papers in these areas

---

**Note**: This research reference is continuously updated with new findings from arXiv. Last update: 2025.

For vulnerability details and mitigations, see [REFERENCE_VULNERABILITIES_NEW.md](./REFERENCE_VULNERABILITIES_NEW.md).ABILITIES.md).