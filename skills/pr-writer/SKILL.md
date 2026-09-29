---
name: pr-writer
description: Draft or review a pull/merge request title and description from actual branch changes. Use when asked for a PR/MR description, reviewer-facing explanation, or help filling a repository PR template; explain the problem and impact before the solution. Never publish or update a PR.
---

# PR Writer

Draft a self-contained explanation that helps a reviewer decide whether the change solves a worthwhile problem and whether the implementation matches its intent. Read the [reviewer-focused writing guide](references/pr-writing-guide.md) for the reasoning behind the structure; use the [minimal body template](templates/pr-body.md) only when the repository has no suitable template.

## Boundaries

**Draft in chat by default.** Do not create or update a PR/MR, push a branch, or use `gh pr create/edit`, `glab mr create/update`, or platform APIs, even if the user asks. Instead, offer a draft for the user to review and publish themselves. Do not write a file unless the user explicitly requests one. These are behavioral instructions, not a technical security boundary on Pi's other tools.

## Establish the proposed PR scope

Determine the intended base branch and what changes the PR will actually contain. Inspect repository instructions and any PR/MR template before imposing a format. Use local Git information without fetching or contacting a host by default:

1. Identify the current branch and status. If the target base is unclear, ask rather than assuming `main` or an outdated remote ref is correct.
2. For committed changes, find the merge base of the chosen base ref and `HEAD`; inspect the diff and stat **from that merge base to `HEAD`**, along with relevant commits. Comparing the two branch tips directly can accidentally include unrelated changes from an advanced base branch. If there is no `HEAD` or merge base, ask for the intended scope or user-supplied context instead of pretending a PR diff exists.
3. Inspect staged, unstaged, and untracked changes separately. A PR based on `HEAD` does not yet include them. Warn if they seem relevant; only fold them into a *prospective* draft when the user explicitly wants that, and label the draft accordingly. For large or generated diffs, inspect enough to establish intent without dumping sensitive data.

For an existing description review, the user may provide the text without a repository; you can improve its clarity, but say when its factual claims cannot be checked against changes. If the diff contains unrelated work, flag the scope and suggest splitting or describing the pieces honestly, not forcing a misleading single-purpose story.

## Write for a reviewer

Start with the motivating problem and its observable impact; for an internal refactor, describe the developer or maintenance problem rather than inventing a user-visible failure. Then explain the new behavior or design in plain English so a reviewer can compare the intent with the diff. Focus on consequential decisions and risks, not a paragraph per file or copied commit messages. Keep the body self-contained: links may provide detail but cannot replace the explanation. If referring to a particular commit, give its short ID **and** subject.

State what was actually validated, based on observed test output or information the user supplied. Distinguish that from optional steps a reviewer can perform. Never fabricate tests, results, benchmarks, root causes, issue links, or affected users. Claims of performance gains need measured numbers and context; note meaningful costs or uncertainty rather than implying every optimization is free. Ask for missing motivation or evidence when it affects the draft; otherwise omit or qualify unsupported claims.

Choose a concise title following the repository's convention (imperative where appropriate), separate from the body unless its template requires a heading. Prefer portable Markdown across GitHub, GitLab, and Bitbucket; don't add GFM-only alerts or collapsible sections unless supported and useful. Include only helpful sections, such as problem/approach, observed verification, and non-obvious trade-offs or migration notes. No fixed line-count target, redundant file-by-file table, or empty headings. Never forge a human's `Signed-off-by` certification. Only add an applicable `Assisted-by` disclosure when explicitly requested; if repository rules require one, surface that requirement for the user to decide.

## Deliver the draft

In chat, show the suggested title and body distinctly, with a brief note about the base and whether the draft covers only committed changes. If no tests ran, say so rather than filling in sample output. If a file is requested, write **the body** to the requested path or an unused project-local `tmp/` path, tell the user where it is, and present the title separately. Check before overwriting any existing draft. Never publish or edit a remote PR on the user's behalf.
