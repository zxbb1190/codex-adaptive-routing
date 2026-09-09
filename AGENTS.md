# Adaptive Multi-Agent Routing Policy

This repository contains a reusable Codex orchestration policy. These rules
are project guidance for the root agent; they do not require every task to use
subagents.

## Coordinator

The root agent is the coordinator and owns the final result. It is explicitly
authorized to proactively delegate to configured subagents when delegation is
likely to improve quality, reduce expensive reasoning, reduce root-context
consumption, or enable useful parallel work. The root agent decides whether to
delegate, which role to use, whether to run work in parallel, and whether an
independent review is worth its cost. It does not wait for the user to name a
subagent.

This is adaptive authorization, not mandatory fan-out. The root agent should
complete trivial work directly when delegation overhead would exceed the value.

## Decision Rules

Before substantial work, evaluate:

1. Can the root agent complete the task cheaply and reliably?
2. Can a cheaper worker complete a bounded part reliably?
3. Are there genuinely independent parts worth parallelizing?
4. Would delegation reduce root-context usage or improve specialist reasoning?
5. Is independent review worth its additional token cost?

Do not create subagents merely because they exist. Avoid delegation for tiny
edits, obvious fixes, simple questions about already-read code, or work that
requires continuous context from the root thread.

Consider delegation for broad exploration, independent subtasks, well-specified
implementation, difficult reasoning, consequential review, or work that can be
done reliably by a lower-cost worker.

## Role Selection

Choose the lowest-cost capable role:

- `luna_scanner`: read-only file, symbol, route, test, usage, and pattern discovery.
- `luna_worker`: mechanical edits, types, lint, formatting, simple tests, and small isolated changes.
- `terra_worker`: default implementation for ordinary features, API integration, UI, CRUD, and routine bugs.
- `sol_worker`: complex implementation, cross-module state, performance, concurrency, migrations, and difficult debugging.
- `sol_reviewer`: independent review for consequential changes, subtle correctness, security, concurrency, and regressions.
- `astra_architect`: selective planning for genuine architecture decisions, new subsystems, major contracts, data models, or major refactors.
- `astra_arbiter`: last-resort escalation when strong workers fail or major architectural uncertainty remains.

## Reasoning Effort

Model capability and reasoning effort are independent decisions.

- `low`: bounded and deterministic work.
- `medium`: ordinary planning and implementation.
- `high`: difficult debugging, subtle correctness, and consequential review.

Do not automatically use `xhigh`, `max`, or `ultra`. Prefer upgrading model
capability before repeatedly increasing reasoning effort.

Default escalation is Luna -> Terra -> Sol. Escalate to Astra only when the
unresolved problem is architectural. Do not repeatedly retry an insufficient
role.

## Parallelism and Completion

The configured concurrency is a ceiling, not a target. Run parallel work only
when tasks are independent, ownership is clear, dependencies are known, and
elapsed time or quality will materially improve. Prefer one or two useful
workers over filling every slot.

Workers must report files changed, behavior changed, validation, and remaining
risk. The root agent integrates results, resolves conflicts, runs proportionate
validation, decides whether review is needed, and returns one coherent result.
