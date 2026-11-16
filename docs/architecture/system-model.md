# System architecture model

This document defines logical responsibilities, not deployment services. Components may initially be modules in one application; the boundaries exist to keep responsibilities and contracts clear.

## Logical components

### 1. Intake and workflow interface
Accepts a user objective or an existing workflow definition. Captures the requested outcome, inputs, constraints, scope, authorization, budgets, and acceptance criteria. It should reject or clarify materially ambiguous requests before execution is authorized.

### 2. Planner
Transforms the authorized request into a structured plan. A plan contains tasks, dependencies, expected outputs, required capabilities, risk/side-effect declarations, resource estimates, and completion criteria.

Planner output is untrusted data. It must be schema-validated and policy-checked before it can be scheduled. The planner may propose revisions during execution, but a revision is not executable until it passes the same validation.

### 3. Policy and authorization evaluator
Evaluates whether the requested plan and each consequential action are permitted by the run's authorization envelope. It is authoritative over the planner and executors. It enforces least privilege, resource scope, action classes, budgets, and configured checkpoints.

### 4. Orchestrator and scheduler
Owns run coordination. It manages task readiness, dependency constraints, concurrency, priorities, deadlines, cancellation, retries, and recovery. It should not perform domain work itself; it delegates work to capabilities.

The scheduler must distinguish hard dependencies from informational relationships. Parallelism is permitted only when task dependencies, resource constraints, and side-effect safety allow it.

### 5. Execution workers
Perform assigned tasks. A worker may be deterministic code, a script runner, an external service, or an agent/model-backed executor. Workers receive scoped inputs and permissions and return structured results, errors, and artifact references.

Workers do not own global run state, expand their own permissions, or mark an overall run successful.

### 6. Capability adapters
Expose tools and external systems through stable contracts. Each capability declares:
- identity and version;
- input and output schema;
- required permissions and resource scope;
- whether it is read-only or has side effects;
- idempotency and reconciliation behavior;
- timeout and cancellation behavior;
- relevant rate, cost, and concurrency limits.

Provider-specific details belong behind adapters rather than in the orchestration core.

### 7. Durable run state and event history
Persists run/task state transitions, attempts, decisions, policy outcomes, checkpoints, execution provenance, and references to inputs and outputs. State changes must be recoverable after process or worker failure.

The exact persistence technology and event-storage pattern remain open.

### 8. Artifact and result store
Stores or references generated files, reports, structured outputs, logs, and other deliverables. Artifacts should be linked to the run and task that produced them, with integrity metadata and access controls.

### 9. Observability and control interface
Shows run status, task progress, blocked reasons, resource consumption, capability calls, errors, outputs, and audit history. It also provides controls such as cancel, pause, resume, and explicitly authorized intervention.

## Core domain model

The initial conceptual model is:

- **Workflow request** — an objective or submitted workflow plus inputs and constraints.
- **Authorization envelope** — the approved scope, permissions, policies, budgets, and limits for a run.
- **Workflow definition** — a reusable or submitted description of tasks, dependencies, inputs, outputs, and conditions.
- **Plan** — a concrete, versioned execution proposal derived from a request/definition.
- **Run** — one execution instance with its own state, policy envelope, and provenance.
- **Task** — a unit of work in a plan.
- **Task attempt** — one attempt to execute a task; retries do not erase prior attempts.
- **Capability** — a callable operation exposed to workers under a contract.
- **Artifact** — an output or evidence object produced or consumed during execution.
- **Checkpoint** — a durable recovery point and/or configured control boundary.
- **Execution event** — an immutable record of a meaningful state transition or decision.

These are logical concepts, not a final database schema.

## Control plane and execution plane

Keep control responsibilities distinct from task execution:

- The **control plane** accepts requests, authorizes scope, validates plans, schedules work, tracks state, and exposes observability.
- The **execution plane** runs tasks through workers and capabilities under scoped permissions.

This separation should permit different worker types and execution environments without moving policy authority into those workers. It does not require separate services in the first implementation.

## Planning and graph execution

A workflow plan is naturally represented as a dependency-aware task graph, but the graph must support more than a static linear list. The design should account for:
- independent tasks that can run concurrently;
- hard prerequisites;
- conditional paths;
- bounded iteration or repeated work;
- task fan-out and fan-in;
- dynamically proposed subplans;
- resource and side-effect conflicts.

The exact workflow-definition language and the extent of dynamic planning are open decisions. A generated plan must never bypass validation because it was produced during an already-authorized run.

## Separation from Meridian

The Workflow Orchestrator is not a generalized mode inside Meridian and is not a shared runtime beneath Meridian. It has its own product model, repository, architecture, implementation, contracts, and roadmap. Reuse of general engineering ideas is acceptable; code-level or schema-level coupling is not.
