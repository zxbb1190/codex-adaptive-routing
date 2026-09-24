# Adaptive Multi-Agent Routing Policy

## Coordinator

The root owns requirements, routing, integration, validation, and the final
result. It may proactively delegate when quality, cost, context savings, or
elapsed time justify coordination overhead. No user request to name a subagent
is required. Complete trivial work and tightly coupled steps directly.

## Routing and Escalation

Choose the lowest-cost capable role using `.codex/agents/*.toml` descriptions
and boundaries. Default bounded implementation to `luna_worker`; select
`sol_worker` directly when complexity warrants it. Use `luna_scanner` for
read-only discovery. Detailed responsibilities belong in the role files.

Luna -> Sol -> Astra specialist is an escalation option, not a required chain.
Task difficulty, file count, or a single failed attempt alone never justify
Astra. Keep difficult implementation, debugging, state, concurrency, performance,
migrations, and execution of large refactors with Sol when goals and contracts
are clear. Sol also resolves routine design choices using established patterns.

Use `astra_architect` only for an unresolved design choice that materially
affects core boundaries, ownership, contracts, data models, or lifecycle and
cannot be answered by existing conventions. State the competing options and
their tradeoffs, or the conflict between a requirement and current assumptions.

Use `astra_arbiter` only when the root has checked the evidence and a consequential
conflict between strong workers remains, or multiple distinct, hypothesis-driven
attempts leave important system behavior unexplained. Difficult root-cause
analysis can qualify without an architecture change. Repeating the same failed
approach does not qualify. Do not manufacture extra attempts to meet this gate.

Before calling Astra, provide the specific unresolved question, why existing
conventions cannot answer it, relevant code and validation evidence, attempts
and results (if any), and the requested decision or explanation. Missing access,
dependencies, or product decisions require resolving that blocker, not a model
upgrade. Pass existing evidence; do not restart broad exploration or repeatedly
retry an insufficient role. The root owns the escalation decision.

Select model capability and reasoning effort independently. Use configured role
defaults; do not automatically increase effort to xhigh, max, or ultra. Prefer
a more capable worker when the current role cannot handle the task.

## Parallelism and Review

Delegate concrete, bounded tasks. Parallel work needs independent scopes, clear
file or module ownership, known dependencies, and a material benefit. Concurrency
is a ceiling, not a target; prefer one or two useful workers over filling every
slot. Tell workers they share the codebase and must preserve others' changes.

Use `sol_reviewer` when independent review is worth its cost, especially for
security, concurrency, migrations, core architecture, or cross-module state.
Routine CSS and straightforward CRUD usually need only proportionate validation.
Review is not a mandatory stage for every task.

## Completion

Workers report files changed, behavior changed, validation results, remaining
risks, and escalation evidence. The root integrates their work, resolves
conflicts, runs proportionate validation, and returns one coherent result.
