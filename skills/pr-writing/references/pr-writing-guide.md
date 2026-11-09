# Explain a change for reviewers

These principles adapt the Linux kernel's [Submitting Patches: describe your changes](https://www.kernel.org/doc/html/latest/process/submitting-patches.html) for pull and merge requests. They also draw on [microsoft/apm's PR-description guidance](https://github.com/microsoft/apm/tree/fef942a2b0125c540b97d4b16fbe3d564fceef86/.agents/skills/pr-description-skill). This is guidance for writing a **draft**, not permission to submit anything.

## Start with a problem, not a tour of the diff

Explain what prompted the work, under what circumstances it matters, and who is affected. A crash, a reproducible regression, an operational limitation, or an awkward developer workflow can all motivate a change. Give a reviewer enough context to judge the need without opening an issue first. If the user hasn't supplied the cause or impact, ask about the important gap; don't infer a dramatic failure from a mechanical diff.

Once the problem is clear, describe the intended behavior and the approach in plain English. State a consequential technical detail when it lets reviewers verify that the code does what the description says. Avoid both vague claims ("improve reliability") and a file-by-file inventory that merely repeats the patch. A title can use the imperative style favored for patches when it suits the repository; the body should read naturally.

## Account for evidence and costs

Describe validation that actually happened: tests, reproduction steps, observed before/after behavior, or the user's reported checks. Distinguish **performed checks** from **instructions for a reviewer to try**. If nothing was run, say "Not run" with the relevant reason rather than inventing console output. Don't claim a benchmark, a fixed race, or a user-visible improvement without support.

When claiming performance, memory, size, or latency improvements, provide measured numbers, workload, and baseline where available. Say what gets worse or more complex as well as what improves. If no measurements exist, report the design expectation as an expectation, not a measured result. Explain rejected alternatives only when the choice is non-obvious; don't add a trade-offs section just to fill space.

## Keep the change coherent and self-contained

A PR can contain multiple dependent commits addressing one coherent goal; it need not literally be a single Linux kernel patch. Point out unrelated fixes or features and suggest splitting when that makes review or validation easier. If it cannot be split yet, describe the contents honestly. Don't rely on a prior PR description, chat history, or a bare URL to explain the current change. When a specific commit matters, include its short ID **and** subject so reviewers know what you're referring to.

The Linux kernel's DCO sign-offs and coding-assistant trailers are not universal PR-body requirements. Never forge a human's `Signed-off-by` certification. Add applicable AI attribution only if requested; if a repository requires it, ask the user how to comply. Likewise, an external example's 150–220-line target, per-file table, and GitHub-only formatting aren't general requirements. Shorten a small PR; let complex work have enough detail to justify its risks and validation.
