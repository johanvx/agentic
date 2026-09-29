# Building Python packages with uv

`uv build` is a build frontend: it builds with the backend already named in `pyproject.toml`. Keep an existing project's backend and layout unless the user asks to change them. Do not equate "using uv" with migrating a project's build system.

## New pure-Python packages

For a new library, `uv_build` is a reasonable starting backend:

```sh
uv init --lib --build-backend uv telemetry-client
uv build
```

The resulting project commonly uses this layout:

```text
telemetry-client/
├── pyproject.toml
└── src/
    └── telemetry_client/
        └── __init__.py
```

The distribution name and import name can differ: hyphens in a distribution name are normally mapped to underscores in its module name. A minimal manual build-system declaration is:

```toml
[build-system]
requires = ["uv_build"]
build-backend = "uv_build"
```

For a published package, choose and test an appropriate bounded `uv_build` version range rather than copying a stale pinned range from an example. Check the current [uv build-backend documentation](https://docs.astral.sh/uv/concepts/build-backend/) before relying on version-specific options.

## Layout changes

The default source root is `src/`. For a different import name or a root-level module, configure it deliberately:

```toml
[tool.uv.build-backend]
module-name = "telemetry_client"
module-root = ""
```

With `module-root = ""`, the package would live at `telemetry_client/` at the repository root instead of under `src/`. For a namespace package `acme.reports`, use `module-name = "acme.reports"` with `src/acme/reports/__init__.py` but no `src/acme/__init__.py`.

`uv_build` excludes Python caches and compiled bytecode by default. Include additional source files only when the distribution needs them, and inspect both the sdist and wheel after building:

```toml
[tool.uv.build-backend]
source-include = ["assets/**"]
source-exclude = ["/dist", "tests/**"]
```

In these patterns, an initial `/` anchors an exclusion at the project root. For native extension modules, choose a backend suited to the language and build system (such as maturin or scikit-build-core); hatchling is not the universal choice. `uv build` can still invoke that backend.
