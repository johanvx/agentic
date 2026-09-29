---
name: uv
description: Prefer uv when running or writing Python scripts, managing Python dependencies or environments, packaging Python projects, or adding Python commands to other skills. Use for Python-related work; check that uv is available, preserve existing project toolchains, and ask the user before any fallback if uv is missing.
compatibility: Requires uv on PATH for Python-related work unless the user authorizes an alternative.
---

# Python work with uv

Use `uv` as the default for **new** Python work, including instructions and scripts written for other skills. The goal is a coherent workflow, not a blanket replacement of commands in established projects.

## Check availability before proceeding

Before Python-related work, check that `uv` is on `PATH` (on a POSIX shell, `command -v uv`). If it is absent, tell the user and **stop the Python-related work**. Wait for them to install `uv` or explicitly decide on a fallback. Do not install it, run `pip` or bare `python`/`venv`, or silently substitute another tool. Even when an alternative seems obvious, the choice belongs to the user.

If `uv` is available, inspect the project's existing Python configuration before choosing commands. Respect its current dependency manager, lockfile, build backend, and CI workflow. Do not migrate an existing project just to enforce this preference; follow an explicit user choice of tooling.

## Choose a workflow

| Situation | Starting point |
|---|---|
| New standalone script, no project dependencies | `uv run --no-project scripts/audit_data.py` |
| Script with ad-hoc dependencies | `uv run --no-project --with httpx scripts/fetch_data.py` |
| Reusable standalone script | Declare PEP 723 inline dependencies; use `uv add --script scripts/fetch_data.py httpx`, then `uv run --no-project scripts/fetch_data.py`. |
| Existing uv-managed project | `uv add httpx` for a requested new dependency, `uv run ...` for commands, `uv sync` for the environment. Check whether syncing or a lockfile update is intended. |
| One-off Python CLI | `uv tool run ruff check .` (or `uvx ruff check .` where `uvx` is available). |
| Explicit virtual environment outside a uv project | `uv venv` and, where appropriate, `uv pip`. Confirm the target before creating a venv: `uv venv` can replace an existing one. |
| Package build | `uv build` uses the project's configured backend. For a **new pure-Python** package, consider `uv_build`; retain the backend of an existing package. |

For script metadata, locks, Python versions, indexes, and portable shebangs, read [running scripts](references/scripts.md). For `uv_build`, package layouts, namespaces, and file inclusion, read [building packages](references/build.md). The reference repository that informed these guides is [agent-stuff's uv skill](https://github.com/mitsuhiko/agent-stuff/tree/main/skills/uv); these instructions are rewritten for this Pi package.

## Apply the preference to other skills

When creating or reviewing a skill that runs Python, check its executable examples, scripts, dependency setup, and stated compatibility. For **new** workflows, show `uv` commands rather than assuming `pip install`, `python script.py`, or `python -m venv`; specify any script dependencies in inline metadata or a project manifest. Explain to the skill's users that `uv` is needed and that they should stop and ask if it is missing, rather than using an automatic fallback.

Do not edit unrelated skills or translate established project commands without a request. `uv run python -m ...` is still the right way to invoke a Python module in many contexts; `uv venv` and `uv pip` are legitimate when a workflow requires an explicit environment. The preference is for `uv` to manage Python work, not a ban on the word `python`.
