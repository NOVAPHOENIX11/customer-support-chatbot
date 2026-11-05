# Repository audit — 2026-11-05

**Scope:** Read-only review of the public `main` branch's tracked file tree and selected text files. This is **not** a comprehensive security test, credential exposure review of image pixels, or a live AWS deployment test. The original application code remains unchanged in this pull request.

## Current implementation (observed)

- Python CLI in `chat.py` invokes an Amazon Bedrock AgentCore **managed harness** and streams replies. The harness config attaches an AgentCore Gateway exposing a `create_bug_report` tool.
- `setup_gateway.py` defines a 3-field bug-report tool schema and provisions an MCP Gateway target backed by Lambda.
- `cloudformation-tool.yaml` defines DynamoDB, Lambda and IAM roles. `create_bug_report.py` mirrors the inlined Lambda handler.
- The handler checks all three required bug-report fields are nonempty and saves DynamoDB items with ticket IDs and timestamps. This is valuable input validation, but it is not authentication, authorization or robust semantic validation.
- `system_prompt.txt` attempts to route requests among bug reports, FAQ questions and unrelated requests.
- `harness-tests.json` contains four reference scenarios. `evaluation_observations.txt` records a 0.75 correctness score on four cases, not a verified production benchmark.

## Priority findings

| Priority | Finding / evidence | Why it matters | Proposed correction |
|---|---|---|---|
| P0 | No source-ownership/licensing determination; project uses educational-course materials | Publishing/selling third-party coursework may breach applicable rights | Review origins of every file. Reimplement independently where necessary before commercial licensing |
| P0 | Public repo tracks `agentcore_config.json`, containing **lab-specific** account/resource ARNs and identifiers, despite listing the file in `.gitignore` | Misleads new users; may expose obsolete infrastructure metadata and invite wrong-account calls | Audit all tracked content/history and images for secrets. Remove active resource configuration from tracked files in a dedicated reviewed change; add a sanitized `agentcore_config.example.json` |
| P0 | No customer identity/authentication or tenant isolation was demonstrated in the reviewed support path | Any future public client service must protect customer data and tool access | Add authenticated API layer, authorization, tenant-scoped data access, least-privilege IAM and audit trail before deployment |
| P0 | Recorded evaluation response claimed a ticket was created when the prompt lacked reproduction/environment information | False confirmation can lead to lost support requests | Server-side collection/validation, explicit tool-success checks, end-to-end tests that verify the DynamoDB record |
| P1 | Recorded outputs include visible `<thinking>` content and internal route labels | Inappropriate internal output to end users | Remove private reasoning from outputs, separate internal instrumentation, add output filtering and regression tests |
| P1 | `requirements.txt` pins boto3 and transitive libraries but no developer/test framework | Difficult onboarding and test execution; future dependency drift | Document Python versions, add isolated fast unit tests, CI workflow and secure dependency policy |
| P1 | No README, no docs, no declared license at audit start | Portfolio visitors cannot judge the project accurately | Add honest README and audit/roadmap via draft PR; defer license until rights review |
| P1 | Reproduction relies on live AWS IAM, AgentCore harness APIs and account-specific configuration | New users cannot reliably demonstrate the system without paid resources and compatible permissions | Introduce local mocks / no-cloud demo; infrastructure variable inputs; cost estimates and teardown instructions |
| P1 | Lambda prints raw request events to CloudWatch | Could leak customer descriptions and environment details in logs | Redact customer fields, minimize collection, set retention and access controls |
| P2 | FAQ is directly injected into a long system prompt | Model context and prompt-injection / token cost risks | Separate trusted instructions from untrusted customer/FAQ content; adopt retrieval and source citation when needed |
| P2 | Demo screenshots and evaluation outputs may contain identifiers or internal data | Public materials need disclosure review | Manually review each image and recorded output, including historical commits |
| P2 | Infrastructure file mirrors Lambda implementation, requiring two edits for one behavior change | Risk of code drift | Package one canonical Lambda implementation or use reproducible build artifact |

## Positive foundations

- End-to-end architecture includes an agent/harness, tool discovery, Gateway, Lambda and DynamoDB.
- Backend handler validates required fields and returns ticket status/ID.
- Least-privilege intent is visible in the Lambda's DynamoDB `PutItem` scope; remaining roles still require independent review.
- Evaluation artifacts document shortcomings rather than solely showcasing successful responses.
- Multi-turn terminal conversation is available through a session ID.

## Security and Git-history review limits

Only one commit was found during this review. Current text inspection found **no obvious literal AWS access key** in the inspected source files. That **does not** certify the repo as secret-free: screenshots, historical objects and uninspected fields require additional validation with a dedicated scanner. Treat embedded account IDs/ARNs as infrastructure metadata, **not access credentials**, but do not publish active operational configuration unnecessarily. If a genuine secret was ever committed, revoke/rotate it first; merely adding `.gitignore` or deleting the latest file is insufficient.

## Recommended gates before claiming production readiness

1. Ownership and license review complete; new code independently implementable and distributable.
2. Repeatable local demonstration and CI tests pass.
3. Server-side bug report validation + output filtering pass positive/negative cases.
4. Authenticated tenant isolation and least-privilege permissions verified.
5. Redacted logs, failure alerts, retry/timeout policies and audit trail verified.
6. Human-review escalation and safe failures documented.
7. Cloud cost estimates and load testing recorded.
8. A pilot with consent, acceptance criteria and rollback plan completes successfully.

## Review policy

Keep `main` untouched until the user reviews the proposed changes. This audit is documentation, **not a finding that the code is safe to sell**.
