# Codex Adaptive Routing

Reusable project-scoped Codex configuration for adaptive multi-agent software
development.

The root agent uses Astra Low as the coordinator and decides whether a task is
better handled directly or delegated. The configuration provides specialized
roles for low-cost exploration, routine implementation, complex implementation,
review, architecture planning, and last-resort arbitration.

## Contents

- `.codex/config.toml`: project defaults and a concurrency ceiling of 3.
- `.codex/agents/`: seven custom roles with explicit model and reasoning settings.
- `AGENTS.md`: model-independent routing and delegation policy.

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
delegate when the expected quality, context, or latency benefit exceeds the
token and coordination cost. Small tasks can remain single-agent. Model
capability and reasoning effort are selected independently.

The default configuration uses the following model family:

| Role | Model | Reasoning |
| --- | --- | --- |
| Root coordinator | `gpt-6-astra` | `low` |
| Default worker | `gpt-5.6-terra` | `medium` |
| Low-cost roles | `gpt-5.6-luna` | `low` |
| Architecture roles | `gpt-6-astra` | `medium` / `high` |
| Review role | `gpt-5.6-sol` | `high` |

Verify that these model IDs are available to your Codex account before use.

## Verification

Run these checks from the copied project:

```powershell
codex --strict-config doctor --summary --ascii
git diff --check
```

The project must be trusted for Codex to load project-scoped `.codex/` settings.
Do not place API keys, provider credentials, or machine-local provider settings
in this repository.
