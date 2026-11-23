# Northstar Customer Support Agent

**AI-Powered Customer Support Automation | n8n + Ollama + Qdrant**

## Project Overview

Northstar Customer Support Agent is a locally hosted AI customer support automation solution built for a fictional UK-based outdoor equipment retailer.

The project demonstrates how businesses can combine AI agents, workflow automation, retrieval-augmented generation (RAG), and structured business data to handle common customer service requests.

The solution is designed to reduce repetitive manual work while maintaining controlled access to customer information and providing a clear escalation path for cases requiring human assistance.

### Business Challenges

Customer support teams frequently spend time handling repetitive inquiries, including:

- Answering questions about shipping, returns, and store policies.
- Checking order status and delivery information.
- Identifying complaints and requests requiring human intervention.
- Creating and tracking support tickets.
- Handling workflow failures without exposing sensitive technical information.

### Solution

The system uses **n8n** to orchestrate customer interactions and business workflows, **Ollama** for locally hosted language model inference, and **Qdrant** for retrieving relevant store policy information.

It also integrates with **Google Sheets** for structured order and support records and **Gmail** for support notifications.

The implementation emphasizes practical business automation, modular workflow design, data access controls, error handling, and human escalation.

**Project Status:** v1.0 — Functional development demo using synthetic customer and order data.

## Key Features

### 1. AI-Powered Customer Support

- Uses a locally hosted Ollama language model (`qwen3:14b`) to interpret customer requests and generate responses.
- Maintains conversational context through shared conversation memory.
- Uses structured instructions to prevent unsupported claims about orders, policies, and support tickets.

### 2. RAG-Based Knowledge Retrieval

- Integrates Qdrant as a vector database for store policy retrieval.
- Uses Ollama embeddings (`nomic-embed-text`) for semantic search.
- Retrieves relevant knowledge before answering questions about returns, shipping, refunds, warranties, and compensation.
- Avoids inventing policies when the knowledge base does not provide sufficient information.

### 3. Verified Order Lookup

- Integrates Google Sheets as a structured source of synthetic order records.
- Requires both an order ID and customer email before performing a lookup.
- Returns order information only when the supplied values match an existing record.
- Prevents disclosure of unrelated customer orders.

**Security note:** Matching an order ID and email is a demo-level access check, not production-grade customer authentication.

### 4. Human Support Escalation

- Identifies complaints and requests requiring human assistance.
- Collects the information required to create a support ticket.
- Requires explicit customer confirmation before ticket creation.
- Checks for existing tickets to reduce duplicate submissions.
- Uses Gmail to notify the support team.

### 5. Error Handling and Recovery

- Uses a reusable error-handling workflow to capture and classify execution failures.
- Provides customer-friendly fallback messages when the AI Agent returns an empty response.
- Separates technical error information from customer-facing responses.
- Supports controlled retries for selected workflow failures.

## System Architecture

The solution uses a modular architecture consisting of one main AI Agent workflow and three supporting workflows.

```text
Customer Chat
      |
      v
n8n Chat Trigger
      |
      v
AI Customer Support Agent
      |
      +---- Ollama (qwen3:14b)
      |
      +---- Conversation Memory
      |
      +---- Qdrant Vector Store
      |        |
      |        +---- Ollama Embeddings
      |
      +---- Order Lookup Tool
      |        |
      |        +---- Google Sheets
      |
      +---- Create Support Ticket Tool
      |        |
      |        +---- Google Sheets
      |        +---- Gmail
      |
      +---- Shared Error Handler
      |
      v
Response Validation
      |
      +---- Valid Response → Customer
      |
      +---- Empty Response → Fallback Message
```

## Workflow Components

| Workflow | Responsibility |
|---|---|
| `northstar-customer-support-agent-local.json` | Main AI Agent, conversation handling, RAG integration, tool orchestration, and response validation |
| `order-lookup.json` | Retrieves matching synthetic order records from Google Sheets |
| `create-support-ticket.json` | Handles confirmed support ticket creation, duplicate checking, and notifications |
| `shared-error-handler.json` | Centralizes workflow error handling and technical failure notifications |

## Setup and Configuration

### Prerequisites

The development environment requires:

- Docker Desktop
- A self-hosted n8n instance
- Ollama with `qwen3:14b` and `nomic-embed-text`
- Qdrant vector database
- Google Sheets and Gmail integrations
- Network connectivity between n8n, Ollama, and Qdrant

### Installation Overview

1. Deploy n8n, Ollama, and Qdrant in the local development environment.
2. Download the four workflow JSON files from the `workflows/` directory.
3. Import the workflows into n8n.
4. Configure the required credentials and Google Sheets resources.
5. Update workflow references, tool connections, and service endpoints to match the target environment.
6. Populate Qdrant with the appropriate store policy documents using a compatible embedding model.
7. Verify the main AI Agent's connections to the three supporting workflows.
8. Run functional tests before enabling the solution for wider use.

**Important:** The exported workflows contain development-specific configuration references. They are not intended to run without environment-specific setup.

## Testing and Validation

The following scenarios were exercised during development:

| Test Scenario | Observed Result |
|---|---|
| Customer policy questions | Successful Qdrant knowledge retrieval in repeated tests |
| Order lookup with matching details | Successful retrieval of matching synthetic order records |
| Order lookup with unmatched details | Order information withheld |
| Support ticket confirmation | Explicit confirmation required before ticket creation |
| Duplicate support ticket request | Existing ticket detected without creating another |
| AI Agent execution failure | Shared error-handling workflow tested |
| Empty AI Agent response | Customer-facing fallback successfully tested |

These results represent development testing rather than a formal production acceptance test.

## Known Limitations

### 1. Intermittent Empty AI Responses

During testing, the locally hosted Ollama model occasionally returned an empty response, even when n8n reported a successful workflow execution.

A fallback mechanism was implemented to prevent empty responses from being displayed to customers.

The underlying cause has not yet been conclusively identified.

### 2. Demo-Level Customer Verification

Order lookup uses a matching order ID and customer email address.

This is suitable for demonstrating workflow logic but does not constitute production-grade customer authentication.

A production implementation would require stronger identity verification and appropriate access controls.

### 3. Synthetic Business Data

All customer, order, and support ticket records used in this project are synthetic.

The solution has not been validated against a live retailer's production systems.

### 4. Environment-Specific Configuration

The workflows rely on locally configured services, credentials, and document references.

Additional configuration and testing are required before deployment in another environment.

## Future Improvements

Potential enhancements include:

- Stronger customer authentication and authorization.
- Automated regression testing for workflow changes.
- Improved monitoring of model response failures and tool execution.
- More robust ticket creation idempotency and recovery mechanisms.
- Evaluation of alternative local and cloud-hosted language models.
- Production-ready deployment configuration and operational documentation.

## Project Disclaimer

Northstar Outdoor Store is a fictional retailer created for demonstration purposes.

This project is an independent technical portfolio demonstration and is not affiliated with a real retail business.

**Version:** 1.0  
**Status:** Functional Development Demo  
**Primary Technologies:** n8n, Ollama, Qdrant, Docker, Google Sheets, Gmail