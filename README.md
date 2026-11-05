# AI Customer Support Chatbot — AWS AgentCore Demo

A **learning and portfolio prototype** for an online-shop support assistant, developed during the Udacity AWS AI & ML Scholars program. It demonstrates grounded FAQ answering, multi-turn bug-report intake, tool use through an Amazon Bedrock AgentCore Gateway, and DynamoDB ticket storage.

> **Status: educational prototype, not production-ready.** It has not been validated with real customer data or independently security-tested. The published evaluation records **0.75 correctness across four test cases**, with known response-safety and workflow shortcomings. This is not a paid-client case study.

## What it demonstrates

- **FAQ support:** answer questions using a fictional online-shop FAQ; hand off unsupported questions rather than inventing policy.
- **Bug-report intake:** request a description, steps to reproduce, and environment before creating a ticket.
- **AWS integration:** an AgentCore managed harness calling a Gateway (MCP), a Lambda function, and a DynamoDB table.
- **Evaluation:** sample tests, dataset-generation scripts, and evaluation observations, with room for improvement.

## Architecture

```text
Customer / terminal (chat.py)
        |
        v
Amazon Bedrock AgentCore managed harness
  |-- Amazon Nova Pro
  |-- system_prompt.txt + demo FAQ
  |
  +--> AgentCore Gateway (MCP)
          |
          v
     AWS Lambda: create_bug_report
          |
          v
       DynamoDB: bug reports
```

The terminal interface uses a session ID for multi-turn conversations. The model can invoke the Gateway tool after gathering required bug information. **Important:** a model prompt alone does not establish authorization or guarantee reliable field validation; production versions must enforce these rules in application code.

## Repository guide

| Path | Purpose |
|---|---|
| `chat.py` | CLI that invokes the managed harness and displays the streamed reply |
| `system_prompt.txt` | Support routing and bug-intake instructions |
| `online_shop_faq.md` | Fictional FAQ used as reference material |
| `cloudformation-tool.yaml` | Example Lambda, DynamoDB and IAM resources |
| `setup_gateway.py` | Creates the AgentCore Gateway and its Lambda target |
| `create_harness.py` | Creates or updates the managed harness |
| `create_bug_report.py` | Source mirror of the Lambda handler embedded in CloudFormation |
| `create_harness.py` / `chat.py` | Harness configuration and interactive test path |
| `harness-tests.json` | Four reference scenarios |
| `evaluation_observations.txt` | Recorded evaluation result and shortcomings |
| `cleanup_agentcore.py` | Destructive AWS cleanup helper: review before running |

The screenshots at repository root are learning-demo evidence; they are **not** client results.

## Reproducing the prototype

Requires Python, AWS CLI credentials, permitted Bedrock model access, and account privileges to deploy IAM roles, Lambda, DynamoDB and AgentCore. **Do not use Udacity lab credentials or identifiers from the tracked `agentcore_config.json` in a new account.**

1. Create a Python environment and install `requirements.txt`.
2. Review the CloudFormation template and AWS charges. Deploy the stack using a **unique stack name** with `CAPABILITY_NAMED_IAM`, only in an AWS account you control.
3. Run `python setup_gateway.py --stack-name <your-stack-name>` to create the Gateway and generate a **new local** `agentcore_config.json`.
4. Run `python create_harness.py` to create the harness.
5. Run `python chat.py` to try multi-turn support conversations.
6. Use `harness-tests.json` and the evaluation scripts to inspect behavior. Verify resulting DynamoDB items and CloudWatch logs before asserting a ticket was created.

These are **source-derived setup steps, not a fresh end-to-end deployment verification**. AWS APIs, permissions, pricing and model availability can change.

## Evaluation and limitations

The repository records a **0.75 automated correctness score over four cases**. Its dataset also shows:
- An agent answer **claiming a bug ticket was created** before the required intake information had been collected.
- Replies exposing internal `<thinking>` markup / route labels.
- At least one weak handoff for out-of-scope requests.

These observations mean the prototype should not be used unsupervised with real customer requests. See [the audit](docs/REPOSITORY_AUDIT.md) and [roadmap](docs/COMMERCIAL_ROADMAP.md).

## Responsible use, ownership, and licensing

Some source structure and material may originate from Udacity coursework. **Do not assume all educational templates, instructions, datasets or screenshots may be redistributed or sold.** Ownership and license review is outstanding. This repository currently **does not declare an open-source license**; no commercial reuse permission is being asserted by this README.

Do not commit credentials, customer information, cloud tokens, or active configuration. Current tracked resource ARNs and evaluation records require review before using this repository for clients.

## Commercial direction

An independently implemented, configurable **NOVA Support & Ticket Automation Kit** could become a small-business service after code ownership, security, correctness, cost, and reliability gates pass. The initial goal is a verified demo and a paid pilot—not a claim that this prototype is already sellable.

See [Commercial Roadmap](docs/COMMERCIAL_ROADMAP.md) for acceptance criteria.

---
**Maintainer:** NOVAPHOENIX11 · **Project type:** educational demonstration · **Production readiness:** not yet.
