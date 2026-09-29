---
name: commit-writer
description: Draft or review Conventional Commit messages from actual Git changes without staging or committing by default. Use when asked for a commit message, a review of one, or a commit; stage and create a commit only when the user explicitly requests it.
---

# Commit Writer

**Drafting is the default.** A request for a message does not authorize `git add` or `git commit`. Even in commit mode, do not push, amend, rewrite history, bypass hooks, or change Git configuration unless separately authorized. Read the [Conventional Commit guide](references/conventional-commit-cheatsheet.md) when choosing a type, scope, body, or trailer.

## Determine the actual candidate changes

For a draft based on repository changes or an actual commit, start with the user's requested scope, `git rev-parse --is-inside-work-tree`, and `git status --short -uall`. Inspect **both** `git diff --cached` and `git diff`; the former is the proposed commit payload, while the latter may contain newer edits to the same files. List relevant untracked files with `git ls-files --others --exclude-standard` and read their content when they are candidates—neither diff shows untracked content. Inspect names and summary first when a file may contain secrets; do not expose sensitive contents just to choose a message. A repository with only untracked files is **not** a no-op.

If `HEAD` exists, inspect a few recent commit subjects for local type and scope conventions. A new repository has no history, so skip this step rather than treating `git log` failure as an error. If the user specified staged changes, draft from the staged snapshot; do not describe unstaged edits as though they were included. If the requested scope is ambiguous, especially with staged, unstaged, and untracked changes mixed together, ask a targeted question or offer clearly labeled alternatives rather than silently combining them. If no relevant changes exist for a diff-based draft, report that rather than inventing a change.

If the user supplies an existing message for review, you can check its wording and format even without repository access or a current diff. State that factual claims cannot be verified without the underlying changes; do not treat a formatting review as permission to stage or commit.

Use only changes and motivations supported by the inspected files or the user's explanation. Do not fabricate test results, issue links, numbers, breaking-change claims, or what a file does. If something matters but cannot be verified, ask or omit it; never put a placeholder or an uncertainty tag in a final commit message.

## Draft a message

Use `<type>[optional scope]: <imperative description>`. Follow the repository's convention where evident; otherwise choose a standard type and omit a doubtful scope. Keep the **whole subject** under 50 characters when possible, without obscuring what changed. Avoid trailing punctuation. Add a body when a non-obvious motivation or trade-off needs explanation: problem first, then solution; wrap prose at 72 characters, not code. Keep the message understandable without opening a link.

Use `!` and a `BREAKING CHANGE:` trailer only for a real, evidenced breaking change; if its migration impact is uncertain, ask. Omit other trailers by default. Add issue, attribution (`Assisted-by`, `Co-authored-by`), or sign-off trailers only when the user explicitly requests them, and never invent names, model versions, or issue numbers. Do not ask about optional trailers on every draft.

For a draft or review request, return the proposed message in a code block, plus a brief note about the files or staged snapshot it describes and any blocking uncertainty. **Do not change the index or create a commit.**

## Commit only on an explicit request

1. Decide the exact logical payload. Show the staging plan and ask when the request leaves room for multiple interpretations; a direct request to commit a clear payload does not require a redundant confirmation. Respect unrelated pre-existing staging: never silently include or unstage it.
2. Check for likely secrets, ignored files, private material, and nested Git repositories before staging. Do not use `git add .`, `git add -A`, or `git commit -a` as a shortcut. Stage only selected paths or hunks; use `git mv` when moving files already tracked by Git. Ask rather than guessing if an untracked checkout might become a gitlink or a partly staged file has additional worktree edits.
3. Inspect the **final staged contents** with `git diff --cached` and the final file set with `git diff --cached --name-status`; check `git status --short` for remaining edits and `git diff --cached --check` for whitespace errors. If unexpected content, possible credentials, or unrelated staged work appears, stop and resolve the scope with the user. Secret checks are best-effort, not a guarantee.
4. Draft from this final staged snapshot. Write the message to a new project-local `tmp/` file **after** staging; keep it out of the payload, run `git commit -F <path>`, and remove the file when done. Do not interpolate message text into a shell command. Never commit placeholders or claims about tests that were not run.
5. If hooks fail, report the failure and inspect the resulting index and worktree before any retry; hooks may have changed files. Do not bypass hooks automatically. After success, report the commit hash and the committed scope. Do not push.

<!-- TODO: A future extension or restricted commit tool could enforce staging/approval boundaries. This skill is guidance, not a technical prohibition on Pi's shell tool. -->
