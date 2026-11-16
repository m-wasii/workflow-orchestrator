# Roadmap

This roadmap sequences validation and architecture work before broad implementation. It is intentionally capability-oriented rather than tied to a language, framework, or vendor.

## Phase 0 — Product definition and constraints

**Goal:** Establish what the first useful product must do and for whom.

Deliverables:
- identify initial customer/workflow profiles and high-value use cases;
- decide objective-driven versus authored workflow entry points for the first release;
- define authorization-envelope requirements and risk tiers;
- establish run/task/plan/artifact contracts;
- decide initial deployment, tenancy, and data-handling constraints;
- define measurable acceptance criteria for the first end-to-end workflow.

Exit condition: one narrow but representative workflow can be specified end-to-end, including permissions, side effects, failure cases, and expected outputs.

## Phase 1 — Minimal durable orchestration core

**Goal:** Prove the run lifecycle independently of any one agent or provider.

Deliverables:
- request, authorization, plan, run, task, attempt, and artifact contracts;
- plan validation and policy enforcement;
- durable task scheduling and dependency handling;
- a minimal worker/capability contract;
- structured results and run history;
- cancellation, timeouts, and basic recovery.

Exit condition: a representative workflow can survive orchestrator/worker interruption without losing run state or blindly duplicating side effects.

## Phase 2 — Reliability and safety foundation

**Goal:** Make execution recoverable, bounded, and trustworthy.

Deliverables:
- retry classification and reconciliation semantics;
- idempotency and stale-worker fencing;
- budgets, quotas, concurrency and circuit breakers;
- scoped credentials and capability-level permission checks;
- audit/provenance records;
- failure injection and recovery test suite.

Exit condition: duplicate delivery, worker loss, timeouts, uncertain outcomes, cancellation, and policy violations have defined and tested behavior.

## Phase 3 — Capability ecosystem and useful workflows

**Goal:** Enable the system to perform meaningful work across external services.

Deliverables:
- versioned capability registry/contracts;
- initial connector/tool adapters selected from validated use cases;
- artifact handling and result delivery;
- user-facing run inspection and control;
- integration tests across multiple capabilities.

Exit condition: multiple end-to-end workflows can be configured without embedding provider-specific logic in the orchestration core.

## Phase 4 — Bounded adaptive autonomy

**Goal:** Allow plans to adapt to execution feedback without weakening policy.

Deliverables:
- plan revision and validation protocol;
- conditional execution and bounded loops;
- subtask decomposition and controlled parallelism;
- policy-aware replanning;
- escalation for ambiguity and out-of-envelope actions;
- evaluation of run outcomes against acceptance criteria.

Exit condition: adaptive execution adds value while every action remains attributable to an accepted plan version and authorization envelope.

## Phase 5 — Production readiness and scale

**Goal:** Prepare for real client workloads under explicit operational commitments.

Deliverables depend on validated product requirements and may include:
- tenant isolation and stronger sandboxing;
- deployment and worker-placement options;
- operational dashboards, alerting, and support workflows;
- backup, retention, and disaster-recovery policies;
- capacity, performance, and cost controls;
- security review and threat modeling;
- versioning, migration, and compatibility policy.

Exit condition: production readiness is demonstrated against defined reliability, security, performance, and support requirements—not inferred from feature completeness.

## Sequencing rule

Do not begin with a large collection of integrations or a sophisticated agent layer. First prove that an authorized run can be planned, validated, durably executed, recovered, and evaluated through a small representative capability set.
