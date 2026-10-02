# Agent-owned interactive sessions

A separate tmux server is useful for a long-running test, debugger, database CLI, or log monitor that the user can observe or reattach to. It is **not** how to reach a user's existing Pi/SSH windows. Keep the session visible to the user and clean up only resources created for the task.

## Create and observe

Use a short, project-local socket path for temporary agent-owned sessions; Unix socket path lengths are limited. These POSIX-shell examples are illustrative—substitute a unique session and socket. Check that the socket is unused before creating a server; if it exists, choose a different path rather than reusing or killing an unknown server:

```sh
mkdir -p tmp
SOCKET="$PWD/tmp/pi-work.sock"
SESSION=pi-work
tmux -S "$SOCKET" -f /dev/null new-session -d -s "$SESSION" -n shell
tmux -S "$SOCKET" list-panes -a -F '#{pane_id} #{session_name}:#{window_index}.#{pane_index}'
```

`-f /dev/null` suppresses tmux configuration only when starting a **new** server. It does not change the pane's login shell; inspect the prompt before sending shell-specific commands. For example, the user's shell might be fish rather than Bash. Avoid creating a new server merely to control an already-authenticated SSH pane. Keep `-S "$SOCKET"` on every command for this server; `-L` takes a socket name, not a filesystem path.

Immediately tell the user how to observe the new session, substituting the **actual** path and session name rather than unexpanded variables:

```sh
tmux -S /absolute/path/to/tmp/pi-work.sock attach -t pi-work
tmux -S /absolute/path/to/tmp/pi-work.sock capture-pane -p -J -t pi-work:0.0 -S -80
```

Use a pane ID discovered with `list-panes` for automation. Only send a command once the intended program's prompt is ready; `tmux wait-for` does not watch pane output. For long-running jobs, periodically capture a bounded amount of output, report progress without dumping sensitive text, and stop polling when the user or program indicates completion. Do not kill the job merely because a short poll timed out.

## Cleanup

Ask before interrupting or closing a user-visible running process. On completion, `tmux -S "$SOCKET" kill-session -t "$SESSION"` is appropriate **only if you created that session**. Never run `kill-server` on a shared server. A detached session can remain intentionally when the user wants to resume it; say so explicitly.

## Run Pi inside tmux

Pi 1.0 defaults to fullscreen: Pi owns the viewport and scrolling rather than relying on normal terminal history. A bounded `capture-pane` shows rendered terminal state, not a complete Pi transcript or reliable tool-result stream. Do not scrape the TUI to reconstruct a conversation or establish that a tool succeeded.

When ordinary terminal scrollback is desired, offer `pi --tui-mode regular` for the next launch. This overrides the mode only for that invocation; do not change the user's default `tuiMode`, interrupt a running Pi, or restart their tmux server to apply it without consent.

For an automated Pi run, prefer explicit `--print` for one final text response or `--mode json` for one-shot JSONL events. Use RPC only when ongoing bidirectional control is needed; see [Pi CLI integration](https://github.com/earendil-works/pi/blob/main/docs/cli-integration.md). `--mode text` alone does not force a one-shot run when stdin and stdout are terminals. Print-mode exit status and JSON events still need interpretation and deliverable checks; pane capture is not a substitute. Use project-local input/log files and avoid exposing sensitive output.

See the [Pi tmux guide](https://github.com/earendil-works/pi/blob/main/docs/tmux.md) for extended-key settings (`Shift+Enter`, etc.). These are terminal configuration concerns, not a way to grant the agent access to remote files. Never restart or kill the user's tmux server just to change keyboard settings without consent.

<!-- TODO: Add a tested socket-aware wait/status helper if manual polling becomes too cumbersome. The [reference skill](https://github.com/mitsuhiko/agent-stuff/tree/main/skills/tmux) always uses the default socket in its waiter and can match old history, so its scripts are intentionally not bundled. -->
