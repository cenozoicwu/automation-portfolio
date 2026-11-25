
# Business Automation & AI Agent Portfolio

**Practical Automation | AI Agent Solutions | Enterprise IT Delivery**

Practical automation and AI-powered solutions designed to streamline business operations, reduce repetitive work, and improve workflow reliability.

This portfolio showcases hands-on projects built with **n8n, AI agents, locally hosted language models, APIs, and business application integrations**, combining technical implementation with over 10 years of enterprise IT delivery experience.

## Featured Projects

| Project | Solution Type | Key Focus |
|---|---|---|
| Sales Lead Automation | Business Process Automation | Data integration, workflow reliability, and error recovery |
| Range Explorer Customer Support Agent v1.2 | AI Agent Solution | RAG, human-in-the-loop authorization, and deterministic business execution |

---

### 1. Sales Lead Automation

**Business Challenge**

Manual lead intake, spreadsheet updates, and follow-up notifications can create delays, inconsistent data, and missed business opportunities.

**Solution**

An n8n-based automation solution that streamlines lead intake, data validation, Google Sheets integration, and email notifications, with additional mechanisms for error handling and recovery.

**Key Capabilities**

- Automated lead intake through webhooks
- Data validation and transformation
- Google Sheets integration
- Automated email notifications
- Retry and recovery mechanisms
- Centralized error handling
- Execution and notification status tracking

**Business Value**

- Reduces repetitive administrative work.
- Improves data consistency and operational visibility.
- Supports recovery from notification failures.
- Demonstrates reliable business process automation beyond basic application connectivity.

**Project Details**

[View Sales Lead Automation](projects/sales-lead-automation/README.md)

---

### 2. Range Explorer Customer Support Agent

![Version](https://img.shields.io/badge/version-1.2-blue)
![Status](https://img.shields.io/badge/status-functional_demo-green)
![Architecture](https://img.shields.io/badge/architecture-hybrid_AI-orange)

**Business Challenge**

Customer support teams frequently handle repetitive inquiries about orders, delivery delays, returns, and store policies. These requests often require information from multiple systems and may involve business actions that require explicit customer authorization.

**Solution**

A locally hosted AI-powered customer support solution built for Range Explorer Outdoor, a fictional UK-based outdoor equipment retailer.

The solution combines conversational AI, retrieval-augmented generation (RAG), structured business data, persistent authorization state, and deterministic workflow execution.

Customers interact with Oliver Bennett, an AI customer support assistant who helps answer policy questions, check order information, identify delivery issues, and prepare support requests for human review.

**Key Capabilities**

- Branded, multi-turn AI customer conversations
- Locally hosted LLM inference using Ollama and Qwen3
- Context-aware RAG using Qdrant
- Section-based knowledge chunking and structured metadata
- Knowledge base versioning
- Order lookup and deterministic delivery status calculation
- Human-in-the-loop support ticket authorization
- Persistent request state and expiration management
- Replay protection and cross-session duplicate detection
- Branded HTML support notifications
- Centralized error handling and fallback responses

**Architecture Highlights**

The solution separates three core responsibilities:

- **AI Reasoning:** Understand customer intent, retrieve relevant information, and prepare business requests.
- **Human Authorization:** Allow customers to review and explicitly confirm proposed actions.
- **Deterministic Execution:** Validate authorization, enforce business rules, and execute approved operations.

This hybrid architecture reduces reliance on LLM-generated decisions for business-critical operations.

**Business Value**

- Supports automation of routine customer inquiries.
- Improves consistency in order and delivery assessments.
- Structures customer issues before human escalation.
- Helps prevent duplicate support requests.
- Preserves customer control over business actions.
- Demonstrates how AI can be integrated into existing operational workflows with appropriate controls.

**Project Details**

[View Range Explorer Customer Support Agent](projects/range-explorer-customer-support/README.md)

---

## Technology Stack

| Category | Technologies |
|---|---|
| Workflow Automation | n8n |
| AI & LLM | Ollama, Qwen3, AI Agents |
| Knowledge Retrieval | RAG, Qdrant, nomic-embed-text |
| Business Integrations | Google Sheets, Gmail, Webhooks |
| State Management | n8n Data Tables |
| APIs & Data | REST APIs, JSON |
| Business Logic | JavaScript |
| Development & Version Control | VS Code, Git, GitHub |
| Deployment | Docker |
| AI Development & Experimentation | Microsoft Foundry |

## Engineering & Delivery Approach

These projects emphasize more than basic workflow connectivity or AI model integration.

My implementation approach combines business understanding, hands-on technical development, and enterprise delivery practices.

| Principle | Implementation Approach |
|---|---|
| Business Requirements | Translate operational problems into practical automation solutions. |
| Solution Design | Select appropriate technologies and define clear system responsibilities. |
| Integration Design | Connect applications, APIs, data sources, and AI services. |
| Reliability | Implement validation, error handling, retries, and recovery mechanisms. |
| AI Governance | Separate AI reasoning from authorized business execution. |
| Human-in-the-Loop | Require explicit confirmation for selected business actions. |
| Security Awareness | Sanitize public workflow templates and separate credentials from implementation artifacts. |
| Maintainability | Organize workflows into modular, reusable, and understandable components. |
| Testing & Validation | Exercise business scenarios, negative paths, and failure-handling behavior. |
| Documentation | Provide architecture diagrams, configuration guidance, and implementation references. |

## Core Solution Design Philosophy

> Use AI where reasoning and flexibility are valuable.
>
> Use deterministic workflows where accuracy, consistency, and control are essential.
>
> Keep humans in control of decisions that require authorization or accountability.

The objective is not simply to introduce AI into existing processes, but to design practical solutions that balance automation, reliability, operational control, and business value.

## About Me

I am an IT Delivery Manager with over 10 years of experience delivering enterprise technology and digital transformation projects across banking, insurance, and retail.

My professional background includes working with international clients on:

- Enterprise IT project and program delivery
- Data platforms and cloud-native solutions
- Data governance and regulatory compliance
- Cross-border technology initiatives
- Agile delivery and cross-functional team management
- Business process improvement
- Technical integration and operational governance

I am extending this enterprise delivery experience into hands-on AI solution development, intelligent workflow automation, and AI agent implementation.

My focus is on bridging the gap between business requirements and technical execution, combining delivery management experience with practical implementation capabilities.

### Professional Certifications

- Project Management Professional (PMP)
- PMI Agile Certified Practitioner (PMI-ACP)
- Microsoft Applied Skills: Get started developing agents in Microsoft Foundry

### Current Focus

- AI-powered business process automation
- AI agent development and tool integration
- Retrieval-augmented generation (RAG)
- Human-in-the-loop AI workflows
- Enterprise AI solution delivery
- Reliable and maintainable business application integration

## Portfolio Status

The projects in this repository are functional development demonstrations using synthetic business data.

Public workflow templates have been sanitized to remove personal credentials and environment-specific resource identifiers.

The solutions demonstrate practical implementation patterns and architectural approaches rather than production-ready commercial systems.

Quantitative business benefits, production scalability, and performance benchmarks have not yet been independently validated.

---

**Understand the business. Design the solution. Deliver with reliability.**
