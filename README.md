# Workflow Orchestrator

A general-purpose platform for autonomous workflow execution.

The Workflow Orchestrator accepts an authorized objective or workflow, turns it into an executable plan, coordinates tools and workers, tracks progress and side effects, and returns resulting outputs with an execution record.

The product is designed as a standalone, general-purpose platform. Its architecture, implementation, contracts, and roadmap are defined within this repository.

## Current status

This repository is at the architecture-definition stage. The initial documents establish product boundaries and system-level requirements. They deliberately do not select a programming language, framework, database, queue, hosting model, or agent provider. Those choices should follow validated requirements and explicit architecture decisions.

## Start here

- [Product vision and scope](docs/product/vision-and-scope.md)
- [System architecture model](docs/architecture/system-model.md)
- [Execution lifecycle](docs/architecture/execution-lifecycle.md)
- [Reliability, security and autonomy](docs/architecture/reliability-and-safety.md)
- [Roadmap](docs/roadmap/roadmap.md)
- [Architecture decisions](docs/decisions/)

## Core design intent

1. Outcome-oriented, not agent-oriented. The product coordinates work toward an authorized result. Agents are one possible execution capability, not the product's whole architecture.
2. Autonomous within explicit boundaries. Once a user authorizes an objective and its policy envelope, execution proceeds without routine step-by-step approval. Out-of-scope, unsafe, ambiguous, or policy-blocked actions pause safely.
3. Durable by design. Runs, task states, decisions, checkpoints, artifacts, and side effects must survive worker and process failures.
4. Observable and explainable. A run should expose what was planned, what happened, which capabilities were used, what changed, and why it stopped.
5. Tool- and model-independent. Capabilities and model providers should be replaceable behind explicit contracts.
6. Safe around real-world side effects. Authorization, least privilege, idempotency, bounded retries, and recovery are core runtime concerns.

## Non-goals for this initial foundation

- Implementing a code-migration engine.
- Locking the product to a particular LLM provider or agent framework.
- Claiming exactly-once execution or guaranteed success for arbitrary workflows.
- Choosing deployment and tenancy models before requirements are established.

See the open questions in [docs/decisions/open-questions.md](docs/decisions/open-questions.md) before committing to implementation-level design.
