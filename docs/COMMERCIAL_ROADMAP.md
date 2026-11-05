# NOVA commercial roadmap — AWS AI agent portfolio

**Goal:** independently built, legally distributable, secure, supportable AI solutions that demonstrate real engineering competence and can be customized for paying clients. The Udacity lab project is **only the starting reference**, not itself a production product.

## Three project tracks

| Track | Origin and prospective offering | First credible deliverable | Commercial condition |
|---|---|---|---|
| **1. Support Chatbot** (this repository) | Course project → **NOVA Support & Ticket Automation Kit** | Local demo with FAQ lookup, safe ticket intake, CI tests, admin-visible ticket history | Authenticate users, tenant separation, audited ticket creation, client-owned FAQ and repeatable deployment |
| **2. AgentCore Support Agent** | Bedrock/Strands/MCP + RAG/memory/tools → **NOVA Knowledge & Operations Agent** | Independently implemented single-agent demo, RAG citations, typed tool contracts, verified order read-only operations | Permissioned integrations, deterministic money handling, clear human approvals for refunds, privacy and cost controls |
| **3. Multi-Agent E-commerce RAG** | Orchestrator with inventory/policy/refund/communication workers → **NOVA Customer Operations Orchestrator** | Small multi-agent demo with policy-backed decisions, structured handoffs, traceable decisions and human-in-the-loop approval | Workflow durability/idempotency, safe rollback, state coordination, load/evaluation results, operational support |

Do **not** market these as three paid client implementations. Fictional demos and course projects should be labeled as such until a real client engagement is independently verified.

## Backwards-planned acceptance stages

### Stage 5 — Paid pilot
- One client-approved scope and set of supported customer intents.
- Pricing with AWS and monitoring costs separated from implementation fees.
- Written acceptance tests and measurable success criteria.
- Client-owned data/services, authorized access and rollback/handover instructions.

### Stage 4 — Production candidate
- Authentication, authorization and isolation across tenants.
- Data minimization and retention, masked logs, permission boundaries.
- Reliable integrations: API authentication, timeouts, retries, idempotency and failure notification.
- Golden test dataset for correct tool selection, refusal, grounding and safe escalation.
- Deterministic validation on money, inventory and irreversible actions.

### Stage 3 — Portfolio demonstration
- 2–3 minute demo recorded with fabricated data.
- Screenshot gallery, clear architecture and test results.
- Public reproducible demo with test doubles or mocked AWS responses.
- One factual case study per project: problem, architecture, implementation, verified tests and limitations.

### Stage 2 — Independent implementation
- Rights review completed, no unlicensed classroom artifacts duplicated.
- Source under explicit chosen license for independently owned portions.
- Config template without resource IDs/secrets.
- Component separation, unit tests, dependency handling, CI and docs.
- Repeatable deployment in a non-Udacity account when AWS usage is appropriate.

### Stage 1 — Understand and audit
- Trace each tool call: user -> agent -> Gateway -> Lambda/API -> record -> response.
- Inspect Git files/history and screenshots for sensitive material.
- Recreate bug report validation in standalone Python before invoking an LLM.
- Master core concepts as you reimplement: `@tool`, JSON schemas, IAM, AgentCore, Lambda, DynamoDB.

## Eight-week learning-to-delivery sequence

| Week | Relevant learning | Engineering checkpoint |
|---|---|---|
| 1 | Python, AWS SDK, tool contracts and structured outputs | Audit, safe Git housekeeping, test harness and baseline |
| 2 | Pydantic, error handling, IAM fundamentals | Independent support chatbot, deterministic ticket validation |
| 3 | AgentCore, Gateway, MCP and APIs | Typed, read-only integration and meaningful error handling |
| 4 | RAG, Knowledge Bases, embeddings and source grounding | Document-grounded single-agent support workflow |
| 5 | Memory, customer context, test isolation and privacy | Context-aware agent with explicit tenant data boundaries |
| 6 | Multi-agent sequencing/parallelism, routing, structured state | Specialist worker and coordinator interfaces |
| 7 | Durable workflow patterns, guardrails, tracing, evaluation | Failure recovery, human approvals, regression suite, cost model |
| 8 | Product packaging, demos, client discovery and support | Three honest case studies, one repeatable pilot-ready offer |

**Learning rule:** Each week, read the source course material, implement one working feature *without copying course code*, write tests, explain the design tradeoff, and record actual evidence. Do not equate a finished course with production readiness.

## First project — specific backlog, in priority order

1. Ownership and secret-history review; check images manually.
2. Remove tracked lab-specific config in a separate change after review; add sanitized example config and better `.gitignore`.
3. Add pytest tests for Lambda missing/valid field behavior (mock DynamoDB); ensure ticket creation is confirmed by a stored record.
4. Separate the ticket-intake state machine from the LLM prompt; do not invoke tools until validated.
5. Add output checks preventing leak of `<thinking>` and internal routing text.
6. Design authentication and tenant-scoped ticket data before hosting for real users.
7. Add local demonstration without AWS credentials.
8. Validate full cloud deployment with disposable resources and budget limits.
9. Package an Upwork demo and one narrow pilot offer.

## Pricing and business judgment

Start with a **custom implementation service**, not a SaaS subscription. Quotes should account for discovery, integration complexity, hosting, testing, training, maintenance, and handoff. Sell only capabilities that have been reproducibly tested. No claimed conversion gains, ticket-deflection percentages, security certifications, or customer testimonials without evidence.

## Decision gates

- **GitHub public:** ownership known, no secrets, accurate README and screenshots.
- **Upwork portfolio:** a repeatable demo, representative tests, honest simulated results.
- **Sellable implementation:** authorization, reliability, cost assessment and support commitments.
- **Live customer pilot:** signed scope/acceptance and controlled deployment.

## Next iteration

After user reviews the documentation PR: perform a focused security/config hygiene PR, add a test-only CI workflow, then implement the first independently owned reusable support backend. Keep each change small and reviewable.
