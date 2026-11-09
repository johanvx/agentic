# Explain a change for reviewers

These principles adapt the Linux kernel's [Submitting Patches: describe your changes](https://www.kernel.org/doc/html/latest/process/submitting-patches.html) for pull and merge requests. They also draw on [microsoft/apm's PR-description guidance](https://github.com/microsoft/apm/tree/fef942a2b0125c540b97d4b16fbe3d564fceef86/.agents/skills/pr-description-skill), and on technical-writing practice: Michael Alley's *The Craft of Scientific Writing* and George Gopen and Judith Swan's "The Science of Scientific Writing" (*American Scientist*, 1990). This is guidance for writing a **draft**, not permission to submit anything.

## Start with a problem, not a tour of the diff

Explain what prompted the work, under what circumstances it matters, and who is affected. A crash, a reproducible regression, an operational limitation, or an awkward developer workflow can all motivate a change. Give a reviewer enough context to judge the need without opening an issue first. If the user hasn't supplied the cause or impact, ask about the important gap; don't infer a dramatic failure from a mechanical diff.

Once the problem is clear, describe the intended behavior and the approach in plain English. State a consequential technical detail when it lets reviewers verify that the code does what the description says. Avoid both vague claims ("improve reliability") and a file-by-file inventory that merely repeats the patch. A title can use the imperative style favored for patches when it suits the repository; the body should read naturally.

## Lead with the point, then layer the detail

Reviewers read in layers. Many see only the title in a list of open PRs; most read the first paragraph before deciding how much attention the PR needs; few read every section. Put the essentials where every reader sees them:

- **Title:** what the PR changes, specifically ("Drop stale cache entries after an update", not "Cache fixes").
- **First paragraph:** the problem, its impact, and the change in two to four sentences, like the abstract of a paper. A reviewer who stops here should know what they are approving.
- **Later sections:** the design, the evidence, the risks, for the reviewer who reads on.

Within each part, a few writing habits make the text faster to read:

- **Known before new.** Start a sentence with what the reader already knows (the component, the symptom just named) and end it with the new information, where readers expect the emphasis.
- **One idea per paragraph,** and headings only when they help a long description; a short PR needs none.
- **Specific words.** "Retries the upload up to three times on HTTP 503" instead of "improves robustness"; "drop", "split", "cache" instead of "handle" or "update".
- **Consistent terms.** Use the codebase's name for a thing every time; switching between "session" and "connection" suggests two different things.
- **No filler.** Cut "This PR", "basically", "in order to"; cut hedges the evidence doesn't need, and keep the ones it does.
- **Lists for parallel items only;** an argument stays prose.

## Account for evidence and costs

Describe validation that actually happened: tests, reproduction steps, observed before/after behavior, or the user's reported checks. Distinguish **performed checks** from **instructions for a reviewer to try**. If nothing was run, say "Not run" with the relevant reason rather than inventing console output. Don't claim a benchmark, a fixed race, or a user-visible improvement without support.

When claiming performance, memory, size, or latency improvements, provide measured numbers, workload, and baseline where available. Say what gets worse or more complex as well as what improves. If no measurements exist, report the design expectation as an expectation, not a measured result. Explain rejected alternatives only when the choice is non-obvious; don't add a trade-offs section just to fill space.

## Separate changes, and guide the reader through a series

Like a kernel patch, a PR is easiest to review when it does one thing. Suggest moving preparatory refactors, file moves, and formatting into an earlier PR, so the main PR's diff shows only the behavior change; keep a bug fix apart from a feature so each can be reverted or backported alone. Each PR should leave the project working.

When one PR holds several commits meant to be reviewed in order, the description plays the role of a kernel cover letter: list the commits with a line on what each does and which depends on which, so a reviewer can go commit by commit. For a large PR, say where to start reading (often the interface or the test that shows the new behavior).

## Keep the change coherent and self-contained

A PR can contain multiple dependent commits addressing one coherent goal; it need not literally be a single Linux kernel patch. Point out unrelated fixes or features and suggest splitting when that makes review or validation easier. If it cannot be split yet, describe the contents honestly. Don't rely on a prior PR description, chat history, or a bare URL to explain the current change. When a specific commit matters, include its short ID **and** subject so reviewers know what you're referring to.

The Linux kernel's DCO sign-offs and coding-assistant trailers are not universal PR-body requirements. Never forge a human's `Signed-off-by` certification. Add applicable AI attribution only if requested; if a repository requires it, ask the user how to comply. Likewise, an external example's 150–220-line target, per-file table, and GitHub-only formatting aren't general requirements. Shorten a small PR; let complex work have enough detail to justify its risks and validation.

## After review

Keep the description an account of the PR as it now is, not of how it got there. When a review round changes the approach, update the description to match, and note what changed since the last review separately, in a short note at the end or a comment, much as kernel patches put version notes below the `---` marker, outside the permanent log.

## Revision checklist

- Do the title and first paragraph alone tell a reviewer what changes and why?
- Is every claim (a fix, a speedup, a test run) backed by the diff, a measurement, or the user?
- Does the description make sense without the issue, the chat, or an earlier PR?
- Does the PR do one thing? If not, is the split proposed or the mix described honestly?
- For several commits, can a reviewer follow them in order?
- Is anything left that only repeats the diff (file lists, per-file narration)?
