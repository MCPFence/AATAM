# AASAM - AI-Centric Agent Threat Analysis Model


![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-1.0.0-green.svg)
![Status](https://img.shields.io/badge/status-Active-brightgreen.svg)

**An AI-centric Agent threat analysis model that helps identify and mitigate security risks in modern AI-driven systems**

[Chinese Documentation](./ZH/AATAM（AI-Centric%20Agent%20Threat%20Analysis%20Model）.md) | [English Documentation](./AATAM%20(AI-Centric%20Agent%20Threat%20Analysis%20Model).md) | [Entity Threat List](./实体威胁列表.md)

</div>

## 📖 Project Overview

AASAM (AI-Centric Agent Threat Analysis Model) is a threat analysis framework specifically designed for AI Agent systems. With the rapid development of AI Agent technology, traditional threat models can no longer effectively address emerging security challenges. This project proposes an AI-centric threat analysis approach to help developers and security researchers identify, analyze, and mitigate security risks in AI systems.

### 🎯 Why AASAM is Needed?

- **Reality-Driven**: Based on numerous real AI security incidents and vulnerability cases
- **Traditional Model Limitations**: Traditional threat models cannot adapt to the complex scenarios of AI Agents
- **Emerging Threats**: AI systems face new types of attacks such as indirect prompt injection and model poisoning
- **Frequent Vulnerabilities**: Well-known AI tools like Cursor frequently expose high-risk security vulnerabilities

## ✨ Core Features

### 🔍 AI-Centric Data Flow Analysis
- **Bidirectional Data Flow**: Analyzes data flows with AI as both endpoint and starting point
- **Entity Classification**: Categorizes system entities into five major types: Model, Environment, Ecosystem, Expansion, Human
- **Threat Modeling**: Systematic threat analysis combined with STRIDE model

### 🛡️ Comprehensive Threat Coverage
- **Model Layer Threats**: Jailbreak, training data poisoning, AI platform poisoning
- **Environment Layer Threats**: Command injection, sandbox escape, unauthorized access
- **Ecosystem Layer Threats**: Supply chain attacks, MCP tool poisoning
- **Expansion Layer Threats**: New manifestations of traditional security vulnerabilities in AI scenarios
- **Human Layer Threats**: Social engineering attacks, misinformation propagation

## 📁 Project Structure

```
AASAM/
├── README.md                    # Main project documentation
├── 实体威胁列表.md                # Entity threat classification list
├── AATAM (AI-Centric Agent Threat Analysis Model).md  # English documentation
└── ZH/                          # Chinese documentation directory
    ├── AATAM（AI-Centric Agent Threat Analysis Model）.md
    └── img/                     # Image resources
        ├── dataflow.jpg
        ├── dataflow1.jpg
        ├── dataflow2.jpg
        └── dataflow3.jpg
```

## 🚀 Quick Start

### 1. Understanding the AASAM Model
- **[Chinese Detailed Documentation](./ZH/AATAM（AI-Centric%20Agent%20Threat%20Analysis%20Model）.md)** - Complete model introduction
- **[English Documentation](./AATAM%20(AI-Centric%20Agent%20Threat%20Analysis%20Model).md)** - English version of the model
- **[Entity Threat List](./实体威胁列表.md)** - Threat classification checklist

### 2. AASAM Methodology Three Steps

#### Step 1: AI-Centric Data Flow Mapping
1. Identify system entities (Model, Environment, Ecosystem, Expansion, Human)
2. Draw bidirectional data flow diagrams centered on Foundation Model

#### Step 2: Threat Analysis
- Input threat analysis: Identify attack vectors
- Output threat analysis: Analyze using STRIDE model

#### Step 3: Risk Mitigation
- Permission restriction strategies
- User authorization mechanisms

## 📊 Threat Entity Classification

| Entity Type | Main Threats |
|-------------|--------------|
| **Model** | Jailbreak attacks, model backdoors, adversarial attacks |
| **Environment** | Command injection, sandbox escape, unauthorized access |
| **Ecosystem** | Supply chain attacks |
| **Expansion** | Traditional security vulnerabilities |
| **Human** | Social engineering attacks, false advertising |

## 💡 Practical Application Cases

### MCP (Model Context Protocol) Threat Analysis
The project uses the currently popular MCP application as an example to demonstrate the application of the AASAM model in detail:

- **Data Flow Analysis**: Complete MCP system data flow diagram
- **Threat Identification**: Discover 20+ specific threat scenarios
- **Attack Paths**: Analyze real attack cases such as Cursor client RCE
- **Mitigation Measures**: Provide specific security protection recommendations

## 🔗 Related Resources

- **GitHub Repository**: [MCPFence/AATAM](https://github.com/MCPFence/AATAM)
- **Reference Models**: STRIDE, PASTA, MAESTRO
- **Related Research**: MCP security threat analysis papers

## 🤝 Contribution Guidelines

We welcome community contributions! You can participate in the following ways:

1. **Report Issues**: Submit Issues to report discovered threats or improvement suggestions
2. **Improve Documentation**: Help improve documentation content and translations
3. **Case Studies**: Share new AI security incidents and threat cases
4. **Tool Development**: Develop automated security tools based on AASAM

## 👥 Contributors

We extend our heartfelt gratitude to all contributors who have helped make AASAM a valuable resource for the AI security community. Your contributions, whether through code, documentation, research, or feedback, are essential to the project's success.

### Core Contributors

| Name | Role | GitHub Profile | Contributions |
|------|------|----------------|---------------|
| *Knight* | Project Maintainer | [@knightswd](https://github.com/knightswd) | Initial framework, documentation |
| *0xEaS* | Security Researcher | [@0xEaS1](https://github.com/0xEaS1) | Threat analysis, case studies |


### How to Become a Contributor

We welcome contributions from security researchers, developers, and AI enthusiasts! Here's how you can get involved:

1. **🐛 Report Security Threats**: Share new AI security incidents or threat patterns
2. **📚 Improve Documentation**: Help with translations, examples, or clarifications
3. **🔍 Research Contributions**: Contribute threat analysis, case studies, or security assessments
4. **💻 Tool Development**: Build automation tools or security utilities based on AASAM
5. **🌍 Community Engagement**: Help spread awareness and gather feedback

**To be added to the contributors list:**
- Make meaningful contributions to the project
- Submit a pull request with your changes
- Include a brief description of your contribution
- We'll add you to the contributors table with your preferred role and GitHub profile

### Recognition

All contributors will be recognized in this README and in our project documentation. We believe in acknowledging every contribution that helps improve AI security.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**AASAM Project** - Safeguarding AI Agent System Security 🛡️
