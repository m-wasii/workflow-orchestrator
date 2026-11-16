# Execution lifecycle

## Run lifecycle

A run moves through explicit, durable phases:

1. **Received** — request and input references are recorded.
2. **Clarifying** — missing information or conflicting constraints are resolved when needed.
3. **Awaiting authorization** — the user confirms the objective and authorization envelope.
4. **Planning** — the planner creates a structured plan and identifies required capabilities.
5. **Validating** — schemas, policy, permissions, resource limits, and acceptance criteria are checked.
6. **Ready** — the plan is valid and tasks can be scheduled.
7. **Executing** — eligible tasks are dispatched and coordinated.
8. **Recovering** — retryable failures, worker loss, or interrupted work are reconciled.
9. **Evaluating** — outputs are checked against task and run acceptance criteria.
10. **Terminal** — the run is completed, partially completed, blocked, failed, or cancelled.

A run may enter a controlled **Paused/Blocked** condition from an active phase when policy, missing authority, unresolved ambiguity, or an unavailable dependency prevents safe progress. Resumption must re-check relevant state and authorization; it must not blindly replay side effects.

## Task lifecycle

A task should have a state model separate from the run:

- pending — known but not yet eligible.
- ready — dependencies and preconditions are satisfied.
- dispatched — assigned to a worker.
- running — execution has started.
- retry_wait — eligible for a bounded retry after delay or reconciliation.
- blocked — cannot safely proceed.
- succeeded — task-level acceptance conditions passed.
- failed — non-retryable failure or retry policy exhausted.
- cancelled — execution stopped by run cancellation or policy.
- skipped — deliberately not executed because a condition/path made it unnecessary.

Every execution attempt is recorded independently. A retry creates a new attempt; it does not overwrite the history of the previous attempt.

## Plan lifecycle

Plans are versioned. Initial planning creates a candidate plan; validation makes a specific version eligible for execution. During a run, a planner may propose a replacement or extension. The orchestrator must:
1. preserve the previously accepted plan version;
2. validate the proposed delta and its effects;
3. ensure it remains within the original authorization envelope;
4. check dependencies and conflicts with completed or active tasks;
5. record why the plan changed;
6. schedule only the newly accepted plan version.

A plan revision cannot retroactively authorize an action or change the evidence for a completed task.

## Scheduling rules

- Dispatch only tasks whose hard prerequisites have reached accepted terminal states.
- Parallelize independent tasks only when resource limits and side-effect semantics permit it.
- Use bounded concurrency, deadlines, attempt limits, and resource budgets.
- Prevent stale workers from committing results after their lease or assignment has expired.
- Reconcile uncertain external side effects before retrying.
- Preserve deterministic ordering where the workflow contract requires it.
- Propagate cancellation and stop launching new work once cancellation is accepted; record tasks that cannot be interrupted immediately.

## Completion semantics

A run is **completed** only when all required tasks and run-level acceptance criteria pass.

Other terminal outcomes must remain distinct:
- **partially completed** — useful outputs exist, but required work remains incomplete;
- **blocked** — safe progress requires additional input, permission, or an unavailable prerequisite;
- **failed** — the run cannot meet its contract under the configured recovery policy;
- **cancelled** — the run was intentionally stopped.

A worker's claim that it finished is not sufficient evidence of task success. The orchestrator evaluates structured results against declared criteria.
