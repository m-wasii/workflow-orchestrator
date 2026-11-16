# Repository guidance for AI coding agents

## Product identity

This repository contains the general-purpose Workflow Orchestrator. It is independent of Meridian. Do not import, copy, extract, or couple to Meridian code, schemas, runtime services, domain model, or deployment artifacts. Meridian-specific C/C++ to Rust migration concepts do not belong here unless a separately justified general-purpose requirement is documented.

## Before implementation

1. Read the README and relevant product/architecture documents.
2. Identify which requirement or decision authorizes the change.
3. Check the architecture decision records and open questions.
4. Prefer small, testable increments and explicit contracts.
5. Do not invent product decisions or silently resolve open architectural questions.

## Engineering expectations

- Treat durable state transitions and external side effects as first-class design concerns.
- Make retries bounded and safe; do not retry non-idempotent actions blindly.
- Enforce permissions at the capability boundary, not merely in prompts.
- Keep planner/model output untrusted until validated against schemas and policy.
- Preserve execution provenance, inputs, outputs, errors, and relevant artifact references.
- Make cancellation, timeout, recovery, and partial completion explicit.
- Separate orchestration policy from individual tool/worker implementations.
- Design for provider and capability replacement; avoid leaking vendor-specific types into core contracts.
- Test failure paths, duplicate delivery, worker loss, stale leases, and recovery—not only happy paths.

## Decision discipline

- Do not select a language, framework, database, queue, deployment model, or agent framework without an accepted decision record.
- If an implementation requires an unresolved product decision, document the options and trade-offs and stop short of encoding an arbitrary assumption.
- Update the relevant docs and decision records when behavior or contracts change.
- Never claim a workflow is verified or successful unless the configured acceptance criteria actually passed.

## Scope and authorization

A workflow may act only within its user-authorized objective, resource scope, permissions, and budgets. If a step exceeds that envelope or cannot be evaluated safely, block or pause the run with a clear reason. No agent may grant itself additional authority.
