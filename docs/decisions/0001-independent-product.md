# ADR 0001: Workflow Orchestrator is an independent product

- **Status:** Accepted
- **Date:** 2026-11-16

## Context

Meridian is a specialized modernization initiative focused on C/C++ to Rust migration. The Workflow Orchestrator is intended to execute general-purpose client workflows. Treating the latter as a second mode inside Meridian or extracting a shared implementation would couple two products with different domains, requirements, and evolution paths.

## Decision

The Workflow Orchestrator is a separate product with its own repository, architecture, domain model, contracts, implementation, and roadmap.

No code, runtime, schema, persistence model, or deployment artifact is to be shared with Meridian. General engineering principles may be learned from either project, but any similar implementation must be designed and justified independently in this repository.

## Consequences

- This repository must not import Meridian-specific migration concepts or dependencies.
- Architecture choices must be made from general workflow requirements and customer use cases.
- Product boundaries remain explicit even if both systems use autonomous planning, task scheduling, workers, or verification.
- Any future proposal to change this separation requires a new decision record and explicit reconsideration.
