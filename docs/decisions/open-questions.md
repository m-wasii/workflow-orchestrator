# Open architecture and product questions

These questions are intentionally unresolved. They should be closed through product discovery, prototypes, and explicit decision records—not guessed by implementation agents.

## Product and workflow model

1. Who is the first target customer and which specific workflow should define the first release?
2. Does a user primarily submit a natural-language objective, author a reusable workflow, trigger a prebuilt template, or use a combination?
3. How much of the workflow may be planned dynamically at runtime versus defined in advance?
4. What should a reusable workflow's versioning, parameterization, and publishing model look like?
5. Which outputs matter most: files, structured records, completed external actions, reports, or a mix?

## Execution model

6. Which task types are required initially: API calls, scripts, browser actions, code execution, agent tasks, scheduled jobs, or event-driven handlers?
7. What execution environments are required, and what isolation level does each task type need?
8. Which workflows require durable waits, timers, callbacks, or external events?
9. What are the required semantics for parallel branches, loops, compensation, and long-running tasks?
10. What is the expected interaction model for pause, resume, cancellation, and exception handling?

## Security and governance

11. Is the first release single-user, single-tenant, or multi-tenant?
12. Which identity providers, credential stores, and authorization models are required?
13. Which action classes require explicit checkpoints, and can customers configure these policies?
14. What data may be sent to model providers, stored in artifacts, or retained in audit history?
15. Which compliance, residency, retention, and deletion requirements apply?

## Platform and operations

16. Should the first deployment be hosted, self-hosted, local-first, or support more than one mode?
17. What availability, recovery-time, recovery-point, latency, and throughput targets matter?
18. What observability should be available to end users versus operators?
19. Which integration ecosystem is most valuable for the first customer segment?
20. What cost model and execution budgets should be exposed to customers?

## Implementation decisions

21. Which language and runtime best fit the validated execution and deployment model?
22. Which persistence, queueing, scheduling, and artifact-storage technologies meet the durability requirements?
23. Should the orchestration core use a workflow engine, a custom state machine, or a combination?
24. Which agent/model providers are needed initially, and what provider abstraction is justified?
25. What testing, evaluation, and security review process is required before client workloads?

## Decision rule

A question is ready for closure when the selected option, alternatives considered, evidence, trade-offs, and consequences can be recorded in an ADR. Do not silently convert an assumption into an architectural commitment.
