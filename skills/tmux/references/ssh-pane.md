# Working through an existing SSH pane

Choose this route when the user wants to reuse an already-authenticated SSH shell, especially if opening another connection would require MFA or a jump host. This route controls terminal input and reads its rendered text; it does **not** redirect Pi's file tools to the remote machine. Follow the permission policy in [SKILL.md](../SKILL.md) before sending commands.

## Locate and verify one pane

When Pi runs inside the user's tmux server, ordinary `tmux` commands use that server. If the server is non-default and Pi cannot see it, ask the user for its socket path (`tmux -S /path/to/socket ...`) or socket **name** (`tmux -L name ...`). Use the same selector on **every** command; `-L` does not accept the path used with `-S`. Do not start a private socket to find windows in the user's existing server.

```sh
tmux list-panes -a -F '#{pane_id} #{session_name}:#{window_index}.#{pane_index} #{pane_current_command} #{pane_title}'
```

Ask the user which pane may be inspected, then substitute its ID for `%7` in the examples below. Never assume the SSH window is `:0.0` or that `pane_current_command` alone proves the remote identity. After the user confirms the pane, inspect it:

```sh
tmux capture-pane -p -J -t '%7' -S -80
```

If its recent output shows a prompt ready for a shell command, send a small read-only identity check:

```sh
tmux send-keys -t '%7' -l -- 'hostname; id -un; pwd'
tmux send-keys -t '%7' Enter
tmux capture-pane -p -J -t '%7' -S -80
```

Verify the observed hostname and user with the user before broader operations. Check that the pane still belongs to the same session/connection each time. Never type into Pi's pane, a pager, a running editor, a password prompt, or an unclear shell state. An SSH reconnect invalidates the previous target binding and any routine-write permission.

## Read text and recognize new output

For small text, send a read-only command literally with `send-keys -l`, send `Enter` separately, then capture bounded recent history. Use command quoting appropriate to the **remote** shell and file names. The pane can echo the command, wrap/truncate long output, and contain old history or terminal control effects; inspect it rather than assuming capture is a complete file or a successful command.

For a simple one-line shell command, append a fresh completion marker to the **same line** before sending it. For example, replace the pane and token below for each invocation:

```sh
tmux send-keys -t '%7' -l -- 'pwd; printf "\n%s%s\n" "__PI_DONE_" "run-91dca60e"'
tmux send-keys -t '%7' Enter
tmux capture-pane -p -J -t '%7' -S -80
```

Poll the same pane for `__PI_DONE_run-91dca60e` with a bounded timeout. The marker is split across two arguments in the typed line, so matching the **joined** marker avoids matching its echoed input. Use a different hard-to-guess token every time; never search for a generic prompt or a match from old scrollback. This was tested with a local fish pane, but shells and interactive applications vary. The marker only shows that the shell reached `printf`; it does **not** prove the preceding command succeeded. If it does not appear, or the shell asks for input, stop and ask rather than sending further commands blindly.

Before a write, preview the exact text to be sent—including any marker or wrapper—and obtain approval according to the current session policy. Afterwards, perform an appropriate read-only verification. If an exact exit status, complete contents, binary data, or large file transfer matters, propose a fresh direct SSH/SFTP transfer or a remote Pi process instead of treating pane scraping as reliable transport; ask before switching away from the existing pane. Never print an entire capture on timeout: it may contain secrets.
