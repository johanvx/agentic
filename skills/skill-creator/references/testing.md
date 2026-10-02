# Testing a skill in Pi

Read this when planning model-based checks or comparing skill revisions. Start
with a few agreed cases, not a benchmark harness. The CLI examples target Pi
1.0; check the installed Skills, Command Line, Security, and JSON Event Stream
docs and `pi --help` before relying on version-specific behavior.

## Define the evidence before spending tokens

Agree on realistic requests, expected behavior, and any paid model runs with
the user. Include valid uses and a near miss. Record the candidate revision,
`pi --version`, the exact provider/model ID, effective thinking level, tool
selection, relevant settings, and fixture inputs. Hold these constant when
comparing revisions; a stochastic model need not repeat its earlier output.

Check names, frontmatter, bundled links, and dependencies first. Discovery and
CLI-interface checks do not establish that a model selects or follows a skill.
Use objective checks for deliverables where possible rather than accepting the
model's own claim that its work succeeded.

## Control context without claiming a sandbox

A plain `pi --skill <candidate>` also leaves other configured resources active.
Name collisions keep the first discovered skill, so a comparison may exercise
a different copy than intended. Start with a controlled resource selection:

- `--no-skills --skill <absolute-candidate-path>` makes only the explicitly
  selected skill available. Check diagnostics for missing paths or collisions,
  and verify the actual skill path in the run's messages or tool calls.
  `--skill` advertises the skill; it does **not** force invocation.
- `--no-context-files` excludes global and project context-file instructions.
  `--no-approve` skips trust-gated project settings and resources rather than
  relying on a saved trust decision.
- `--no-extensions`, `--no-prompt-templates`, and `--no-themes` remove those
  extra resources. Disabling extensions also disables built-in MCP, codemode,
  and llama.cpp support. If the test needs an extension, review it, load only
  that one explicitly with `-e`, and record the changed test environment.
- `--no-session` uses a fresh, unpersisted conversation. It does not remove
  files written by tools or logs you save yourself. Do not resume another run
  when comparing candidates.

User-level settings, credentials, and user `SYSTEM.md`/`APPEND_SYSTEM.md`
overrides can still affect these runs. Record relevant overrides. If a clean
agent directory is necessary, agree on authentication before using
`PI_CODING_AGENT_DIR` with a separate project-local test directory; do not copy
the user's private credential store or print resolved tokens into test logs.

Use a fresh case workspace under the project's `tmp/`, with disposable,
non-sensitive fixtures. Write request files with Pi's `write` tool and pass
paths rather than piping ad-hoc input. Choose unused log paths and restore
fixtures to the same starting state before each candidate run.

These flags control context and tool selection, **not operating-system
permissions**. Even read-only tools can read accessible paths outside the case
workspace. Shell tools, extensions, and configured credential commands can
have side effects. Use an approved OS sandbox/container when access must be
restricted. `--offline` suppresses automatic network activity such as catalog
refreshes; it does not prevent a requested model call or sandbox tool traffic.

## Separate selection from forced execution

The examples below are for an approved read-only case. Set `SKILL_DIR` to the
candidate's absolute directory and `TEST_MODEL` to the agreed exact
`provider/model-id`; do not assume a provider has usable authentication. Use
existing authorized authentication without putting secrets in CLI arguments.
Run from the prepared case workspace containing `request.md` and its fixtures.

### Automatic selection

Do not name the skill or tell the model to load it in `request.md`. This checks
whether the description routes an ordinary request, including the near miss:

```sh
SKILL_DIR=/absolute/path/to/skill-under-test
TEST_MODEL=provider/model-id

pi --offline --no-approve --no-context-files \
  --no-extensions --no-skills --skill "$SKILL_DIR" \
  --no-prompt-templates --no-themes --no-session \
  --model "$TEST_MODEL" --thinking medium \
  --tools read,grep,find,ls --mode json \
  @request.md > automatic.events.jsonl 2> automatic.stderr.log
```

Choose a thinking level supported by the model; Pi clamps unsupported levels.
The read-only selection is suitable only when the case needs no shell command,
conversion, or file creation. For such workflows, agree on the needed tools
and use disposable fixtures; do not remove required tools and call the ensuing
failure a skill defect. `--tools` replaces the selection, so list every needed
tool. These examples deliberately do not enable codemode or connect services.

### Forced workflow

Use the actual frontmatter name for `SKILL_NAME`, with the same candidate,
model, tools, request, and initial fixtures. This bypasses selection so a failed
trigger can be distinguished from instructions that fail when loaded:

```sh
SKILL_NAME=example-skill

pi --offline --no-approve --no-context-files \
  --no-extensions --no-skills --skill "$SKILL_DIR" \
  --no-prompt-templates --no-themes --no-session \
  --model "$TEST_MODEL" --thinking medium \
  --tools read,grep,find,ls --mode json \
  "/skill:$SKILL_NAME Read request.md and carry out its request." \
  > forced.events.jsonl 2> forced.stderr.log
```

Keep the slash command at the beginning of the CLI message, followed by a
space before its arguments. In Pi 1.0, `@file` text is prepended to that message,
so neither putting `/skill:name` inside `@request.md` nor combining that command
with an attachment reliably forces expansion. The example instead directs the
model to read the request by path. Verify the expanded skill block in the user
message; an unknown name can pass through as ordinary text. Record the input
method as part of the case rather than treating these two deliveries as
identical prompts. A skill with `disable-model-invocation: true` is intentionally
explicit-only and should not be expected to pass automatic-selection checks.

After the controlled checks, try the intended installed environment when
worthwhile: isolation can hide competition with other skill descriptions or
dependence on personal instructions. Report those integration results
separately; do not change global settings merely to make a test pass.

## Inspect completed events and artifacts

JSON mode is JSONL progress, not a promise of one JSON answer. Keep stdout for
the event stream and stderr for diagnostics. Read records split on LF; inspect
completed `message_end` messages rather than treating streaming deltas as full
snapshots.

- In an automatic case, check whether a tool actually reads the intended
  `SKILL.md` before applying it. A correct answer alone does not prove that the
  skill loaded; distinguish a selection miss from a workflow failure.
- Inspect `tool_execution_end.isError`, completed assistant `stopReason` values,
  diagnostics, and actual output artifacts. A recovered tool error need not
  fail the case, but record consequential errors and recovery rather than
  hiding them behind a successful final response.
- `agent_end` can be followed by retries or queued work. `agent_settled` marks
  the end of automatic work, **not** successful task completion. An absent
  final boundary can indicate an interrupted/incomplete run.
- JSON mode can exit zero despite a model error or abort; also inspect the
  events. In print mode, use explicit `--print` for a one-shot run and check its
  exit status, but the final text alone cannot establish tool behavior.
- Protect logs as task data: they may contain prompts, file contents, tool
  arguments, and secrets. Keep them local and review before sharing.

Report the cases actually run, observed selection and workflow behavior,
validation performed, and remaining uncertainty. Do not turn a static review
into a claimed model test or a single successful run into a reliability score.

An SDK/RPC harness is intentionally not included: use the CLI until repeatable
batching or bidirectional control justifies another integration layer.
