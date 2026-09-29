# Conventional Commit guide

See the [Conventional Commits specification](https://www.conventionalcommits.org/) for the format. Use repository-specific conventions where they exist; the subject-length and body-wrapping preferences below are this skill's style choices, not requirements of the specification.

```text
<type>[optional scope][!]: <imperative description>

[optional explanation]

[optional trailer]
```

| Type | Use for |
|---|---|
| `feat` | New behavior |
| `fix` | Bug fix |
| `docs` | Documentation-only changes |
| `style` | Formatting without behavior changes |
| `refactor` | Internal restructuring without a feature or bug fix |
| `perf` | Performance improvement supported by evidence |
| `test` | Tests added or updated |
| `build` | Build or dependency tooling |
| `ci` | CI configuration |
| `chore` | Maintenance not covered above |
| `revert` | Reverting a commit |

A scope is a short noun such as `parser`, `ui`, or `deps`; omit it when it does not improve clarity. Prefer a specific subject with the **whole line** under 50 characters, and no final period. Write `add`, `fix`, or `remove` rather than `added` or `fixes`. Do not turn a scoped header into a misleadingly vague one just to meet a length limit.

Only add a body when the reason or design trade-off would otherwise be unclear. Describe the problem before the solution. Wrap prose at 72 columns; leave literal code, paths, and commands intact. Do not claim a test was run unless it was observed.

A `!` in the header marks a breaking change. A real breaking change can also use a `BREAKING CHANGE:` trailer explaining what users must change; verify that impact before writing it. All other trailers, including `Closes #123` and `Assisted-by`, require the user's explicit request. Supplying an issue number alone is not permission to add a trailer.

Examples (illustrative, not claims about this repository):

```text
feat(parser): add CSV header validation

fix(cache): drop stale lookup entries

Updated records could leave an obsolete value in the lookup cache.
Invalidate that entry when a record changes so later reads see the new
value.

feat(api)!: remove v1 endpoints

BREAKING CHANGE: Clients must use the v2 endpoints.
```
