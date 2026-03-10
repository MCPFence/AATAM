---
name: aatam
description: Threat model AI agent apps to find vulnerabilities, analyze attack paths, and generate actionable security checklists for pentests, reviews, and risk assessments.
dependencies: python>=3.8
---

# AATAM - Agent Application Threat Analysis Model

## Overview

AATAM is an entity-centric framework for threat modeling AI Agent applications. It replaces layer-based views with a unified Core Entities model so architecture, data flows, and risks are analyzed together per entity and across their interactions. It helps you identify vulnerabilities, analyze attack paths, and generate actionable security checklists using current research (2023–2026).

Use this skill for:
- Security reviews and pentests of agentic systems
- Threat modeling of LLM-driven agents and tools
- Risk assessments across users, environments, expansions, and ecosystems
- Attack path analysis and mitigation planning

## Quick Start

When analyzing an Agent application, follow this workflow:

1. Clarify scope and objectives (comprehensive vs targeted)
2. Map Core Entities and trust boundaries (Agent, Environment, Ecosystem, Expansion, User)
3. Trace data flows across entity edges (inbound to Agent, outbound from Agent)
4. Apply STRIDE to inbound edges, HARMS to outbound effects
5. Score risks with DREAD and prioritize
6. Generate the entity-centric checklist and attack paths

## Core Components

### Threat Scenario Framework

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

### Entity Relationship Model

```
User ──Use──> Agent
                │
                ├─Drive─> LLM (provides intelligence)
                │
                └─Influence─> Environment <──> Agent (feedback loop)

Agent ──Extend Boundary──> Expansion ──Depend──> Ecosystem

Ecosystem contains multiple single-Agent systems that can collaborate
```

**Core Entities:**
1. **AI Agent** - Autonomous agent driven by large language models
2. **Environment** - External world or system where the agent operates
3. **Ecosystem** - External ecosystem that the agent depends on
4. **Expansion** - External extensions for capabilities, functionality, and knowledge acquisition
5. **User** - Human users interacting with the agent system
(LLM capabilities are analyzed as part of the AI Agent entity; do not model as a separate layer)

## Analysis Workflow

### Step 1: Define Objectives

| Assessment Method | Advantages | Disadvantages | When to Use |
|-------------------|------------|---------------|-------------|
| **Comprehensive Risk Assessment** | Deep analysis; discovers maximum potential risks | Complex process; time-consuming | Critical systems; high-risk applications; compliance requirements |
| **Targeted Risk Assessment** | Lower cost; adaptable to business dynamics | Focuses only on specific risks | Specific threat investigation; rapid assessment; iterative development |

### Step 2: Data Flow Analysis (Entity edges and trust boundaries)

Focus on inbound edges to the Agent (from User, Expansion, Ecosystem) and outbound edges from the Agent (to Environment, Expansion, Ecosystem, User). For each edge, note:
- Trust boundary crossed
- Data types and schemas
- Authentication/authorization mode
- Side effects (file write, network call, state updates)

Apply models:
- STRIDE on inbound-to-Agent edges
- HARMS on outbound-from-Agent effects to users/environment

### Step 3: Risk Assessment

Use DREAD to score each threat on each entity edge. Sum 5 factors (1–10) to prioritize:
- Damage, Reproducibility, Exploitability, Affected Users, Discoverability

Risk bands:
- Critical 40–50, High 30–39, Medium 20–29, Low 10–19

## Core Entities

1) AI Agent — Autonomous agent powered by an LLM (planning, memory, tools)
   - Architecture: prompts/system policy, planning/execution loop, memory (short/long), tool/use of code, connectors
   - Attack surface: prompt/context, tool calls, file I/O, code exec, connectors/APIs, memory state
   - Key threats: prompt/indirect injection, jailbreak/EoP, memory poisoning, data exfiltration, credential leakage
   - Mitigations: capability-scoped tools, approval gates, content and policy guards, output/tool schema hardening, OS/container sandboxing, least privilege, secrets isolation, audit

2) Environment — External systems the agent observes/acts on (files, OS, services, network, devices)
   - Attack surface: file system, shell/OS, HTTP/SSRF, DBs, cloud APIs, browsers, devices/robots
   - Key threats: SSRF/RCE, lateral movement, tool abuse/RCE， quota exhaustion, persistence/state corruption, sensitive data exposure
   - Mitigations: sandbox (FS, process, network), egress allowlists, read-only defaults, per-tool credentials, rate limits/quotas, tamper-evident logs

3) Ecosystem — Third-party platforms and infrastructure the solution depends on (model providers, API hubs, CI/CD, package registries)
   - Attack surface: SDKs/clients, hosted endpoints, webhooks, package/dependency chain, model routers
   - Key threats: supply-chain poisoning, dependency confusion/typosquatting, token leakage, version downgrade, endpoint drift/mode changes
   - Mitigations: SBOM and pinning, checksum/signature verification, provenance/attestation, secret rotation, allowlist providers, inbound/outbound schema validation

4) Expansion — Extensible capability surface (plugins/skills, MCP servers, RAG/retrievers, custom tools)
   - Attack surface: extension install/update, manifests, tool definitions, retriever sources, prompt templates
   - Key threats: malicious extension, arbitrary file write/read, sandbox escape via tool, RAG indirect prompt injection/data poisoning, overbroad permissions
   - Mitigations: signed artifacts and manifest review, explicit permission manifests, per-extension sandboxes, retrieval allowlists/sanitizers, content scanning and deduping, staged rollout

5) User — Human operators and end-users
   - Attack surface: free-form input, uploads, override controls, approvals, feedback
   - Key threats: social engineering, toxic/unsafe content, consent/privacy violations, privilege escalation via approvals, prompt disclosure of secrets
   - Mitigations: RBAC and separation of duties, structured input with validation, redaction/PII minimization, safe output filters, high-risk action approvals with context stripping, audit trails

## Entity-Centric Vulnerability Quick Reference

### AI Agent
- Prompt/indirect injection; jailbreak/EoP; memory poisoning; tool abuse/RCE; secret leakage; unsafe code execution

### Environment
- SSRF/RCE; lateral movement; state corruption and persistence; data exfiltration; quota exhaustion

### Ecosystem
- Dependency confusion/typosquatting; SDK/client misconfig; token leakage; version downgrade; webhook abuse

### Expansion
- Malicious extension; arbitrary file read/write; sandbox escape via tool; RAG indirect prompt injection; data poisoning; overbroad permissions

### User
- Social engineering; unsafe outputs; privacy/consent violations; approval misuse; secret disclosure via prompts

**For detailed vulnerability catalogs with attack vectors, mitigations, and research references, see:**
- [REFERENCE_VULNERABILITIES_NEW.md](./REFERENCE_VULNERABILITIES_NEW.md) - Entity-centric vulnerability database organized by core entities (AI Agent, Environment, Ecosystem, Expansion, User) and components
- [REFERENCE_RESEARCH_NEW.md](./REFERENCE_RESEARCH_NEW.md) - Academic research from arXiv (2023-2026) with attack success rates and defense strategies

## Output Format (use entity-centric sections)

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

- Path: Ecosystem:API -> Agent -> Environment:HTTP -> Internal Service
  - Risk: High (DREAD 36)
  - Mitigation: Egress allowlist, SSRF guard, schema validation

### 3. References
- Vulnerability category: [REF] Entity:Expansion/RAG/Indirect Injection
- Research: arXiv IDs where applicable
```

## Execution Steps

When using this skill:

1. **Ask clarifying questions** about the Agent system architecture:
   - Single agent or multi-agent system?
   - What components? (LLM, tools, memory, environment)
   - What data sources/sinks?
   - What user interactions?
   - Which Core Entities are present and where are trust boundaries? (AI Agent, Environment, Ecosystem, Expansion, User)
   - What expansion mechanisms? (MCP, Skills, A2A)

2. **Set assessment scope** based on business requirements:
   - Comprehensive assessment for critical systems
   - Targeted assessment for specific threats
   - Entity-specific focus if needed

3. **Map all entities** using the entity relationship model:
   - Identify all agents, users, environments, expansions
   - Document data sources and sinks
   - Map trust boundaries

4. **Create data flow diagrams** for both directions:
   - Agent as endpoint (attack vectors from external entities)
   - Agent as source (malicious behaviors toward external entities)

5. **Apply threat models** systematically:
   - Use STRIDE for attack vectors (Agent as target)
   - Use HARMS for malicious behaviors (Agent as attacker)
   - Reference vulnerability database for known issues
   - Consult research papers for latest attack vectors

6. **Score and prioritize risks** using DREAD or CVSS:
   - Calculate DREAD scores for each identified threat
   - Prioritize based on risk level
   - Consider research-backed attack success rates

7. **Generate actionable checklist** with specific mitigations:
   - Reference appropriate vulnerability documentation sections
   - Include research-based mitigations with proven effectiveness
   - Provide implementation guidance

8. **Cite references** from the vulnerability database and research papers:
   - Link to specific vulnerability categories in REFERENCE_VULNERABILITIES_NEW.md
   - Reference relevant arXiv papers with findings
   - Include case studies if applicable

## Reference Documentation

### Primary References
- **[REFERENCE_VULNERABILITIES_NEW.md](./REFERENCE_VULNERABILITIES_NEW.md)** - Comprehensive vulnerability database organized by:
  - Core entities (AI Agent, Environment, Ecosystem, Expansion, User)
  - Component-specific vulnerabilities (MCP, Skills, A2A, Gateway)
  - Attack vectors, mitigations, severity ratings, and research references

- **[REFERENCE_RESEARCH_NEW.md](./REFERENCE_RESEARCH_NEW.md)** - Academic research compilation from arXiv (2023-2026) mapped to core entities and attack paths:
  - Tool/supply-chain poisoning affecting Expansion and Ecosystem
  - Agentic web threats and governance architectures impacting AI Agent and Environment
  - Memory attacks and cross-agent flows impacting AI Agent and Ecosystem
  - Defense mechanisms with quantified effectiveness (93–98.5% interception rates)
  - Multi-agent system vulnerabilities

- (Optional) Case studies and attack examples
  - Data Analysis Agent security scenarios
  - Attack chain analysis with step-by-step breakdowns
  - Practical mitigation examples with implementation guidance

### Supporting References
- **STRIDE Threat Model** - Microsoft's threat categorization framework
- **DREAD Risk Assessment** - Microsoft's risk scoring methodology
- **OWASP Top 10 for LLM Applications** - Industry-standard security guidelines
- **MITRE ATLAS** - Adversarial threat landscape for AI systems

## Key Research Findings (2023–2026)

- Supply chain/tool poisoning is prevalent in extensions and skills
- Indirect prompt injection affects RAG and browsing agents widely
- Memory poisoning creates cross-session persistence risks
- Governance layers can substantially reduce but not eliminate risk; defense-in-depth remains necessary

## Best Practices

### When to Use This Skill
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

### Integration with Other Frameworks
- **OWASP LLM Top 10**: Map AATAM findings to OWASP categories
- **MITRE ATLAS**: Correlate with adversary tactics and techniques
- **NIST AI RMF**: Align with risk management framework
- **ISO/IEC 27001**: Integrate with information security management

## Skill Dependencies

This skill may use Python scripts for:
- Automated threat modeling visualization
- Risk score calculation (DREAD/CVSS)
- Checklist generation
- Attack path diagram generation

Ensure Python 3.8+ is available in the execution environment.

## Version and Updates

- **Version**: 2.0
- **Last Updated**: 2025
- **Research Coverage**: arXiv papers from 2023-2026 (75+ papers)
- **Vulnerability Coverage**: 50+ documented vulnerabilities across core entities
- **Defense Coverage**: Research-backed mitigation strategies with effectiveness metrics

## Contributing

To extend this skill with new vulnerabilities or research:
1. Add new entries to REFERENCE_VULNERABILITIES_NEW.md with complete metadata
2. Update REFERENCE_RESEARCH_NEW.md with recent papers including attack success rates
3. Add case studies demonstrating new attack vectors
4. Update severity ratings based on new research findings
5. Include arXiv references for traceability

---

**Note**: This skill follows the progressive disclosure pattern. The SKILL.md file provides core workflow and quick reference. Detailed vulnerability catalogs, research findings, and case studies are available in the reference files and will be loaded as needed during analysis.