# Reliability, security and autonomy

## Authorization envelope

Every run must be bound to an explicit authorization envelope established before execution. At minimum, it defines:
- the objective and permitted outcome;
- allowed resources, accounts, repositories, systems, or data scopes;
- permitted action classes and required capability permissions;
- prohibited actions and protected resources;
- cost, token, compute, rate, concurrency, and time budgets as applicable;
- configured approval checkpoints;
- data handling and retention constraints;
- acceptance criteria and stop conditions.

The envelope is authoritative. Prompts and agent-generated plans cannot expand it. Each consequential capability invocation must be checked against the effective permissions and scope.

## Least privilege and credential handling

- Issue short-lived or otherwise bounded credentials where practical.
- Scope credentials to the resources and operations needed for the task.
- Do not expose broad secrets to planners or unrelated workers.
- Keep secrets out of prompts, logs, artifacts, and error messages.
- Record the identity and scope of the capability used without recording secret values.
- Revoke or expire credentials when a run or task no longer needs them.

## Side-effect safety

External calls may succeed even when the worker times out or loses its connection. Therefore, a timeout does not prove that an action did not happen.

For each capability, define its side-effect semantics:
- read-only;
- idempotent write;
- write with an idempotency key;
- write with a reconciliation/read-after-write path;
- non-idempotent or irreversible operation.

Retries are bounded and permitted only when safe under the capability's declared semantics. For uncertain outcomes, reconcile state before deciding whether to retry. Do not claim exactly-once execution across arbitrary external systems.

Compensation is not universal rollback. Where a capability supports compensation, define it explicitly, including failure modes and limits. A compensation attempt must itself be authorized and audited.

## Durable execution and recovery

The runtime must preserve enough state to recover from:
- process restart;
- worker crash or lost connection;
- duplicate message or task delivery;
- lease expiration and stale worker completion;
- transient service failure;
- rate limiting;
- partial workflow completion;
- cancellation during active work.

Use durable state transitions, attempt histories, checkpoints, bounded retries, backoff, circuit breakers, and dead-letter/blocked handling as appropriate. State transitions should be idempotent. Worker leases or fencing tokens should prevent stale attempts from committing results after reassignment.

The storage implementation is undecided, but in-memory-only run state is insufficient for production use.

## Isolation and tenant boundaries

The product may eventually execute client workflows across different trust domains. The architecture must leave room for tenant isolation, per-tenant policy, data separation, scoped credentials, and worker sandboxing. Exact tenancy and deployment choices remain open; no implementation may assume that one customer's data or credentials are available to another.

## Auditability and provenance

Record enough information to reconstruct:
- who or what initiated and authorized a run;
- the request and authorization-envelope version;
- plan versions and validation outcomes;
- task assignments, attempts, and state transitions;
- capability identities, permission scopes, and invocation metadata;
- decisions, policy evaluations, errors, and retries;
- artifacts and output integrity references;
- completion evaluation and the reason for terminal status.

Audit logs must avoid secret leakage and comply with the product's eventual data-retention policy.

## Verification and trust

Use deterministic checks wherever possible: schema validation, policy evaluation, API response validation, artifact integrity checks, business-rule assertions, and explicit acceptance tests. Model-based judgments may supplement these checks but must be labeled as such.

Do not use a single agent's self-review as the sole acceptance gate for consequential actions or completed runs. Verification requirements should be proportional to impact and risk.

## Operational controls

The runtime needs bounded execution time, resource quotas, concurrency limits, cancellation, pause/resume, health monitoring, and operator-visible failure reasons. Any emergency stop should prevent new side effects and clearly report in-flight operations whose final outcome is uncertain.
