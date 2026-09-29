# Codex Adaptive Routing

Reusable project-scoped Codex configuration for adaptive multi-agent software
development.

The root agent uses GPT-6 Sol xhigh as the coordinator and decides whether a task
is better handled directly or delegated. This quality-first configuration favors
strong routing judgement, first-pass quality, and thorough selective review.
It provides specialized roles for discovery, ordinary implementation, complex
implementation, review, architecture planning, and last-resort arbitration.

## Contents

- `.codex/config.toml`: project defaults and a concurrency ceiling of 3.
- `.codex/agents/`: six custom roles with explicit model and reasoning settings.
- `AGENTS.md`: routing, delegation, and escalation policy.

## Use In A Project

Copy `.codex/` and the adaptive routing section from `AGENTS.md` into a trusted
Codex project. Keep that project's own engineering rules in its root
`AGENTS.md`; replace its validation commands, ownership boundaries, security
rules, and release rules as needed.

```powershell
Copy-Item -Recurse .codex <your-project>\
Copy-Item AGENTS.md <your-project>\AGENTS-codex-routing-template.md
```

Then merge the routing policy into the target project's `AGENTS.md` instead of
overwriting project-specific instructions.

## Design

The role files are a resource pool, not a mandatory workflow. The root agent may
delegate when quality, context isolation, parallelism, risk, or elapsed time
justify it. Short or tightly coupled tasks can remain single-agent. The goal is
stronger results with fewer retries, not minimum token consumption. Model
capability and reasoning effort are selected independently.

The default configuration uses the following model family:

| Role | Model | Reasoning |
| --- | --- | --- |
| Root coordinator | `gpt-6-sol` | `xhigh` |
| `luna_scanner` (read-only discovery) | `gpt-6-luna` | `low` |
| `luna_worker` (default implementation) | `gpt-6-luna` | `high` |
| `sol_worker` (complex implementation) | `gpt-6-sol` | `high` |
| `sol_reviewer` (read-only review) | `gpt-6-sol` | `xhigh` |
| `astra_architect` (read-only planning) | `gpt-6-astra` | `medium` |
| `astra_arbiter` (read-only arbitration) | `gpt-6-astra` | `high` |

These are three models and six subagent roles, plus the root coordinator.
Unnamed subagents default to GPT-6 Luna high. Luna handles bounded ordinary
features, UI, API integration, CRUD, tests, and small refactors. Sol handles
complex state, concurrency, performance, migrations, and difficult debugging.
The root may route directly to the appropriate specialist; Luna -> Sol -> Astra
is not a required sequence. Review and architecture work are used selectively.

Luna handles bounded and ordinary execution. Sol handles primary coding,
reasoning, and coordination. Astra is reserved for genuine architecture
decisions and last-resort arbitration. Review is selective, not a required
step after every worker task; ordinary CSS, copy, and simple CRUD usually need
only proportionate validation. The concurrency limit of 3 is a ceiling.

The model family is described in the [official model guidance](https://developers.openai.com/api/docs/guides/latest-model).
The role boundaries and concurrency ceiling here are project policy.

Verify that these model IDs are available to your Codex account before use.

## Apply To An Existing Project

Replace the routing template files and merge `AGENTS.md` with your project's
engineering rules. Remove obsolete role files from the target project: copying
new files alone does not remove files that were previously installed. Check
project-local escalation references against the six current role names.

Start a new Codex task after upgrading so it can load the new project settings
and role definitions. Existing tasks may retain their loaded configuration.

## Verification

Run these checks from the copied project:

```powershell
codex --strict-config doctor --summary --ascii
git diff --check
```

The project must be trusted for Codex to load project-scoped `.codex/` settings.
Do not place API keys, provider credentials, or machine-local provider settings
in this repository.
