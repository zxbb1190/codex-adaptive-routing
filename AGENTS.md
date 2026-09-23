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
Route genuine architecture decisions directly to `astra_architect`. Reserve
`astra_arbiter` for consequential unresolved issues after strong workers fail
or their evidence conflicts. Pass existing evidence rather than restarting
exploration. Do not repeatedly retry an insufficient role.

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
