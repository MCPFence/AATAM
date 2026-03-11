# AATAM - Agent-Centric Application Threat Analysis Modeling

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-2.0.0-green.svg)
![Status](https://img.shields.io/badge/status-Active-brightgreen.svg)

**An entity-centric framework for threat modeling AI Agent applications to identify vulnerabilities, analyze attack paths, and generate actionable security checklists**

[Chinese Documentation](./aatam/SKILL_CN.md) | [English Documentation](./aatam/SKILL.md) | [Vulnerability Database](./aatam/REFERENCE_VULNERABILITIES_NEW.md) | [Research Database](./aatam/REFERENCE_RESEARCH_NEW.md)

---

## 📖 Project Overview

AATAM (Agent Application Threat Analysis Model) is an entity-centric framework specifically designed for threat modeling AI Agent applications. It replaces layer-based views with a unified Core Entities model so architecture, data flows, and risks are analyzed together per entity and across their interactions. This framework helps security researchers and developers identify vulnerabilities, analyze attack paths, and generate actionable security checklists using current research (2023–2026).

### 🎯 Why AATAM is Needed?

- **Based on Real-world Research**: Compiled from 75+ arXiv papers (2023-2026) with quantified attack success rates and defense effectiveness metrics
- **Entity-Centric Analysis**: Unified model analyzing AI Agent, Environment, Ecosystem, Expansion, and User entities together
- **Research-Backed Defenses**: Mitigation strategies with proven effectiveness (93–98.5% interception rates)
- **Practical Application**: Designed for security reviews, pentests, threat modeling, and risk assessments of agentic systems

### 🎯 Use Cases

- **Security Reviews & Pentests**: Comprehensive security assessments of agentic systems
- **Threat Modeling**: LLM-driven agents and tools threat modeling
- **Risk Assessments**: Evaluating risks across users, environments, expansions, and ecosystems
- **Attack Path Analysis**: Identifying and analyzing potential attack vectors
- **Mitigation Planning**: Developing actionable security recommendations

## ✨ Core Features

### 🔍 Entity-Centric Data Flow Analysis

- **Core Entities Model**: Five major entity types - AI Agent, Environment, Ecosystem, Expansion, User
- **Bidirectional Data Flow**: Analyzes data flows with Agent as both endpoint and source
- **Trust Boundaries**: Maps trust boundaries across entity interactions
- **Threat Modeling**: Systematic analysis using STRIDE (inbound) and HARMS (outbound) models

### 🛡️ Comprehensive Threat Coverage

#### AI Agent Entity
- **Threats**: Prompt/indirect injection, jailbreak/EoP, memory poisoning
- **Attack Surface**: Prompts/context, tool calls, file I/O, code execution, connectors/APIs, memory state

#### Environment Entity
- **Threats**: SSRF/RCE, lateral movement, state corruption, data exfiltration, quota exhaustion
- **Attack Surface**: File system, shell/OS, HTTP/SSRF, databases, cloud APIs, browsers, devices

#### Ecosystem Entity
- **Threats**: Supply-chain poisoning, dependency confusion/typosquatting, token leakage, version downgrade, webhook abuse
- **Attack Surface**: SDKs/clients, hosted endpoints, webhooks, package/dependency chain, model routers

#### Expansion Entity
- **Threats**: Malicious extension, arbitrary file read/write, sandbox escape, RAG indirect prompt injection, data poisoning
- **Attack Surface**: Extension install/update, manifests, tool definitions, retriever sources, prompt templates

#### User Entity
- **Threats**: Social engineering, unsafe outputs, privacy/consent violations, approval misuse, secret disclosure
- **Attack Surface**: Free-form input, uploads, override controls, approvals, feedback

## 🚀 Quick Start

### 1. Understanding the AATAM Model

- **[English Documentation](./SKILL.md)** - Complete model introduction and workflow
- **[Chinese Documentation](./SKILL_CN.md)** - Chinese version of the model
- **[Vulnerability Database](./REFERENCE_VULNERABILITIES_NEW.md)** - 50+ documented vulnerabilities
- **[Research Database](./REFERENCE_RESEARCH_NEW.md)** - 75+ arXiv papers (2023-2026)

### 2. AATAM Methodology

#### Step 1: Define Objectives & Scope

| Assessment Method | Advantages | Disadvantages | When to Use |
|-------------------|------------|---------------|-------------|
| **Comprehensive Risk Assessment** | Deep analysis; discovers maximum potential risks | Complex process; time-consuming | Critical systems; high-risk applications; compliance requirements |
| **Targeted Risk Assessment** | Lower cost; adaptable to business dynamics | Focuses only on specific risks | Specific threat investigation; rapid assessment; iterative development |

#### Step 2: Entity Mapping & Data Flow Analysis

1. **Identify Core Entities**: Map AI Agent, Environment, Ecosystem, Expansion, User
2. **Document Trust Boundaries**: Define trust boundaries across entity interactions
3. **Trace Data Flows**: 
   - Inbound edges to Agent (from User, Expansion, Ecosystem)
   - Outbound edges from Agent (to Environment, Expansion, Ecosystem, User)
4. **Apply Threat Models**:
   - STRIDE on inbound-to-Agent edges
   - HARMS on outbound-from-Agent effects

#### Step 3: Risk Assessment & Prioritization

Use DREAD scoring (1-10 for each factor, sum for total):
- **D**amage Potential
- **R**eproducibility
- **E**xploitability
- **A**ffected Users
- **D**iscoverability

**Risk Bands:**
- Critical: 40–50
- High: 30–39
- Medium: 20–29
- Low: 10–19

## 📊 Entity Relationship Model

```
User ──Use──> Agent
                │
                ├─Drive─> LLM (provides intelligence)
                │
                └─Influence─> Environment <──> Agent (feedback loop)

Agent ──Extend Boundary──> Expansion ──Depend──> Ecosystem

Ecosystem contains multiple single-Agent systems that can collaborate
```

## 💡 Threat Scenario Framework

| Variable | Definition |
|----------|------------|
| **M** | Large Language Model-driven Agent |
| **A** | Attacker's input |
| **B** | Agent's output |
| **Impact(X, S)** | Function evaluating impact of input X on system S |
| **T** | Attacker's target objective |

**Attack Scenarios:**
- **Targeted Attack**: Argmax(Impact(M(A), T)) - Maximize impact on specific target
- **Non-Targeted (Agent Loss of Control)**: Argmax(Impact(B, S)) - Agent generates outputs maximizing system impact

## 🔍 Entity-Centric Vulnerability Quick Reference

### AI Agent
- Prompt/indirect injection; jailbreak/EoP; memory poisoning

### Environment
- SSRF/RCE; lateral movement; state corruption and persistence; data exfiltration; quota exhaustion

### Ecosystem
- Dependency confusion/typosquatting; SDK/client misconfig; token leakage; version downgrade; webhook abuse

### Expansion
- Malicious extension; arbitrary file read/write; sandbox escape via tool; RAG indirect prompt injection; data poisoning; overbroad permissions

### User
- Social engineering; unsafe outputs; privacy/consent violations; approval misuse; secret disclosure via prompts

## 📋 Security Assessment Checklist Format

```markdown
## Security Assessment Checklist

### 1. Entity Findings

#### 1.1 AI Agent
- [ ] Tool capability scope and allowlists in place
- [ ] Prompt/indirect injection guards and output schema validation
- [ ] Memory poisoning protections and context segregation
- [ ] Jailbreak/EoP detection and blocked capabilities

#### 1.2 Environment
- [ ] Process/FS/network sandbox with egress allowlists
- [ ] Read-only defaults; least-privilege per tool
- [ ] Rate limits/quotas and SSRF/RCE guards

#### 1.3 Ecosystem
- [ ] Version pinning/SBOM; signature checks
- [ ] Provider allowlists and secret rotation
- [ ] Webhook/auth hardening; schema validation

#### 1.4 Expansion
- [ ] Signed extensions; manifest review and permissions
- [ ] Per-extension sandbox; RAG source allowlist and sanitization
- [ ] Content scanning/redaction for retrieved data

#### 1.5 User
- [ ] RBAC; approval workflow for high-risk actions
- [ ] Structured inputs and validation; PII minimization
- [ ] Safety filters for outputs; audit trails enabled

### 2. Attack Paths
- Path: User -> Agent -> Expansion:RAG -> Environment:FS
  - Risk: Critical (DREAD 42)
  - Mitigation: Input/retrieval sanitizers, read-only FS, approvals

### 3. References
- Vulnerability category: [REF] Entity:Expansion/RAG/Indirect Injection
- Research: arXiv IDs where applicable
```

## 🔬 Key Research Findings (2023–2026)

- **Supply chain/tool poisoning** is prevalent in extensions and skills
- **Indirect prompt injection** affects RAG and browsing agents widely
- **Memory poisoning** creates cross-session persistence risks
- **Governance layers** can substantially reduce but not eliminate risk; defense-in-depth remains necessary
- **Defense effectiveness**: 93–98.5% interception rates for research-backed mitigations

## 🔗 Related Resources

- **STRIDE Threat Model** - Microsoft's threat categorization framework
- **DREAD Risk Assessment** - Microsoft's risk scoring methodology
- **OWASP Top 10 for LLM Applications** - Industry-standard security guidelines
- **MITRE ATLAS** - Adversarial threat landscape for AI systems
- **NIST AI RMF** - AI risk management framework
- **ISO/IEC 27001** - Information security management

## 🛠️ Integration with Other Frameworks

- **OWASP LLM Top 10**: Map AATAM findings to OWASP categories
- **MITRE ATLAS**: Correlate with adversary tactics and techniques
- **NIST AI RMF**: Align with risk management framework
- **ISO/IEC 27001**: Integrate with information security management

## 📝 Best Practices

### When to Use This Framework
- Before deploying new agent systems
- During security architecture reviews
- When evaluating third-party agent tools or MCPs
- For compliance assessments
- After security incidents involving agents
- When designing multi-agent systems

### Common Pitfalls to Avoid
- Focusing only on a single entity/component and ignoring cross-entity interactions and trust boundaries
- Overlooking indirect prompt injection vectors through external data
- Underestimating supply chain risks from tools, skills, and MCPs
- Ignoring multi-agent interaction risks
- Neglecting client-side security
- Assuming sandboxing is sufficient without proper configuration

## 🤝 Contribution Guidelines

We welcome community contributions! You can participate in the following ways:

1. **Report Issues**: Submit Issues to report discovered threats or improvement suggestions
2. **Add Vulnerabilities**: Contribute new vulnerability entries to REFERENCE_VULNERABILITIES_NEW.md
3. **Add Research**: Update REFERENCE_RESEARCH_NEW.md with recent papers including attack success rates
4. **Improve Documentation**: Help improve documentation content and translations
5. **Case Studies**: Share new AI security incidents and threat cases

### How to Contribute

To extend this framework with new vulnerabilities or research:
1. Add new entries to REFERENCE_VULNERABILITIES_NEW.md with complete metadata
2. Update REFERENCE_RESEARCH_NEW.md with recent papers including attack success rates
3. Add case studies demonstrating new attack vectors
4. Update severity ratings based on new research findings
5. Include arXiv references for traceability

## 👥 Contributors

We extend our heartfelt gratitude to all contributors who have helped make AATAM a valuable resource for the AI security community. Your contributions, whether through code, documentation, research, or feedback, are essential to the project's success.

### Core Contributors

| Name | Role | GitHub Profile | Contributions |
| --- | --- | --- | --- |
| *Knight* | Project Maintainer | [@knightswd](https://github.com/knightswd) | Initial framework, documentation |
| *lcwj3* | Project Maintainer | [@lcwj3](https://github.com/lcwj3) | Initial framework, documentation |
| *0xEaS* | Security Researcher | [@0xEaS1](https://github.com/0xEaS1) | Threat analysis, case studies |

### How to Become a Contributor

We welcome contributions from security researchers, developers, and AI enthusiasts! Here's how you can get involved:

1. **🐛 Report Security Threats**: Share new AI security incidents or threat patterns
2. **📚 Improve Documentation**: Help with translations, examples, or clarifications
3. **🔍 Research Contributions**: Contribute threat analysis, case studies, or security assessments
4. **💻 Tool Development**: Build automation tools or security utilities based on AATAM
5. **🌍 Community Engagement**: Help spread awareness and gather feedback

## 📊 Project Statistics

- **Version**: 2.0
- **Research Coverage**: arXiv papers from 2023-2026 (75+ papers)
- **Vulnerability Coverage**: 50+ documented vulnerabilities across core entities
- **Defense Coverage**: Research-backed mitigation strategies with effectiveness metrics (93-98.5%)

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**AATAM Project** - Safeguarding AI Agent Applications 🛡️
