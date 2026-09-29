# Running Python scripts with uv

Use these patterns for **new** scripts. For a project with its own tooling, inspect that workflow first rather than silently changing environments or lockfiles. If `uv` is absent, stop and get the user's decision before any Python fallback.

## Run without changing a project

```sh
uv run --no-project scripts/check_config.py --verbose
uv run --no-project --python 3.12 scripts/check_config.py
uv run --no-project --with httpx scripts/fetch_release.py
uv run --no-project python -m ast scripts/check_config.py >/dev/null
```

`--no-project` avoids discovering and syncing a surrounding uv project; it does **not** mean Python never uses an already active or discoverable virtual environment. A `.py` file is run as a script. `python -m ast` checks syntax without compiling the target to `__pycache__`; don't confuse it with running the script's tests. Choose a Python version only if the task requires it; uv may need to obtain that interpreter.

## Declare reusable script dependencies

For an existing standalone script, add [PEP 723](https://packaging.python.org/en/latest/specifications/inline-script-metadata/) metadata with `uv add --script scripts/fetch_release.py httpx`. The script then includes a block like:

```python
# /// script
# requires-python = ">=3.11"
# dependencies = ["httpx>=0.27,<1"]
# ///

import httpx
```

Run it with `uv run --no-project scripts/fetch_release.py`. Use `uv init --script scripts/fetch_release.py --python 3.11` only to **create a new file**, not to reinitialize a user's existing script. For a one-off run without changing script metadata, use `uv run --no-project --with httpx scripts/fetch_release.py` instead. For multiple temporary dependencies, repeat `--with`.

`uv add --script ... --index https://packages.example.org/simple ...` can record an alternate index in inline metadata. Do not put private-index credentials in a script or repository; agree on secure credential handling first.

## Reproduce, share, and invoke

`uv lock --script scripts/fetch_release.py` creates an adjacent `.lock` file when script dependencies need a stable resolution. Keep that file with the script if reproducibility matters. In a uv project, prefer `uv run --locked ...` for a checked-in, up-to-date `uv.lock`; unlike `--frozen`, this fails when the lockfile needs updating. Avoid adding a lockfile for a trivial stdlib-only script.

On POSIX systems an executable script can start with `#!/usr/bin/env -S uv run --script`; the user still needs `uv` available. For portable invocation, show `uv run --no-project path/to/script.py` explicitly. Do not assume `env -S` works on every platform.
