---
name: commit-writing
description: Draft or review Conventional Commit messages, and plan how to split work into logical commits, from actual Git changes without staging or committing by default. Use when asked for a commit message, a review of one, how to split changes into commits, or a commit; stage and create commits only when the user explicitly requests it.
---

# Commit Writing

**Drafting is the default.** A request for a message does not authorize `git add` or `git commit`. Even in commit mode, do not push, amend, rewrite history, bypass hooks, or change Git configuration unless separately authorized. Read the [Conventional Commit guide](references/conventional-commit-cheatsheet.md) when choosing a type, scope, body, or trailer, and the [guide to describing and separating changes](references/describing-and-separating.md) when writing a body or when the changes may need more than one commit.

## Determine the actual candidate changes

For a draft based on repository changes or an actual commit, start with the user's requested scope, `git rev-parse --is-inside-work-tree`, and `git status --short -uall`. Inspect **both** `git diff --cached` and `git diff`; the former is the proposed commit payload, while the latter may contain newer edits to the same files. List relevant untracked files with `git ls-files --others --exclude-standard` and read their content when they are candidates—neither diff shows untracked content. Inspect names and summary first when a file may contain secrets; do not expose sensitive contents just to choose a message. A repository with only untracked files is **not** a no-op.

If `HEAD` exists, inspect a few recent commit subjects for local type and scope conventions. A new repository has no history, so skip this step rather than treating `git log` failure as an error. If the user specified staged changes, draft from the staged snapshot; do not describe unstaged edits as though they were included. If the requested scope is ambiguous, especially with staged, unstaged, and untracked changes mixed together, ask a targeted question or offer clearly labeled alternatives rather than silently combining them. If no relevant changes exist for a diff-based draft, report that rather than inventing a change.

If the user supplies an existing message for review, you can check its wording and format even without repository access or a current diff. State that factual claims cannot be verified without the underlying changes; do not treat a formatting review as permission to stage or commit.

Use only changes and motivations supported by the inspected files or the user's explanation. Do not fabricate test results, issue links, numbers, breaking-change claims, or what a file does. If something matters but cannot be verified, ask or omit it; never put a placeholder or an uncertainty tag in a final commit message.

## Separate logical changes

Before drafting, decide whether the candidate changes are **one logical change**. One commit should do one thing that a reviewer can understand, justify, and revert on its own; a change that touches many files can still be one thing. Signs of several changes: the subject needs "and", the body describes unrelated problems, or the diff mixes kinds of work, such as a bug fix with a feature, a refactor with a behavior change, formatting with logic, or a file move with edits to the moved code.

When the changes are several, propose a **split plan** instead of one message: an ordered list of commits, each with its files or hunks and a draft subject. Order them so that each commit builds and passes the tests on its own, with preparatory refactors and moves before the changes that use them, and say when a later commit depends on an earlier one. Keep generated files with their source, and a test with the change it tests. Don't split so finely that a commit cannot be justified alone. If the user wants a single commit anyway, write one honest message that names each part rather than a subject that hides some of them.

## Draft a message

Write for a maintainer reading `git log` or `git blame` years from now, without the conversation, the issue tracker, or the pull request. Use `<type>[optional scope]: <imperative description>`. Follow the repository's convention where evident; otherwise choose a standard type and omit a doubtful scope. The subject is the one claim the commit makes: say what changes, specifically enough to tell it apart from its neighbors in `git log --oneline` (not a file name, not "update" or "fixes"). Keep the **whole subject** under 50 characters when possible and under 72 always, without obscuring what changed. Avoid trailing punctuation.

Add a body when the reason, the impact, or a trade-off is not obvious from the subject and diff. Order it as **problem, impact, solution**: what was wrong or missing and under what circumstances, who or what it affects, then what the change does in plain English, in enough technical detail that a reviewer can check the code against it. Quantify claimed gains with measured numbers, the workload, and the baseline, and name what gets worse. Quote the key line of an error message or log when people would search for it, trimmed of timestamps and noise. Refer to another commit by at least 12 characters of its hash **and** its subject, for example `commit 0123456789ab ("fix(cache): drop stale lookup entries")`. Keep the message self-contained: a link may add detail but cannot replace the explanation, and "as discussed" or "v2" means nothing in the history. A body that keeps growing is a sign the commit should be split.

Write plainly: one idea per paragraph; familiar context before new information; specific verbs and nouns; the same term for the same thing throughout; no filler such as "This commit" or "basically". Wrap prose at 72 characters, not code, paths, or quoted errors.

Use `!` and a `BREAKING CHANGE:` trailer only for a real, evidenced breaking change; if its migration impact is uncertain, ask. Omit other trailers by default. Add issue, attribution (`Assisted-by`, `Co-authored-by`), or sign-off trailers only when the user explicitly requests them, and never invent names, model versions, or issue numbers. Do not ask about optional trailers on every draft.

For a draft or review request, return the proposed message in a code block, or the split plan with one message per commit, plus a brief note about the files or staged snapshot it describes and any blocking uncertainty. **Do not change the index or create a commit.**

## Commit only on an explicit request

1. Decide the exact logical payload. Show the staging plan and ask when the request leaves room for multiple interpretations; a direct request to commit a clear payload does not require a redundant confirmation. For a split plan the user approved, create the commits one at a time in the planned order, repeating steps 2–5 for each. Respect unrelated pre-existing staging: never silently include or unstage it.
2. Check for likely secrets, ignored files, private material, and nested Git repositories before staging. Do not use `git add .`, `git add -A`, or `git commit -a` as a shortcut. Stage only selected paths or hunks; use `git mv` when moving files already tracked by Git. Interactive staging (`git add -p`) needs a terminal the agent does not have: to stage some hunks of a file, write them as a patch in a project-local `tmp/` file, check it with `git apply --cached --check`, then apply it with `git apply --cached`. Ask rather than guessing if an untracked checkout might become a gitlink or a partly staged file has additional worktree edits.
3. Inspect the **final staged contents** with `git diff --cached` and the final file set with `git diff --cached --name-status`; check `git status --short` for remaining edits and `git diff --cached --check` for whitespace errors. If unexpected content, possible credentials, or unrelated staged work appears, stop and resolve the scope with the user. Secret checks are best-effort, not a guarantee.
4. Draft from this final staged snapshot. Write the message to a new project-local `tmp/` file **after** staging; keep it out of the payload, run `git commit -F <path>`, and remove the file when done. Do not interpolate message text into a shell command. Never commit placeholders or claims about tests that were not run.
5. If hooks fail, report the failure and inspect the resulting index and worktree before any retry; hooks may have changed files. Do not bypass hooks automatically. After success, report the commit hash and the committed scope; for a series, say which commits were built or tested on their own and which were not. Do not push.

<!-- TODO: A future extension or restricted commit tool could enforce staging/approval boundaries. This skill is guidance, not a technical prohibition on Pi's shell tool. -->
