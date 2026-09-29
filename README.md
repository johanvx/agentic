# Agentic

skills, extensions, and other agent-related files for agents, mainly [Pi]

[Pi]: https://github.com/earendil-works/pi

## Install

```sh
pi install git:github.com/johanvx/agentic
pi list
```

For local development, run `pi install .` from the repository root instead.
Local-path installs read files in place; reload an active Pi session
with `/reload` after editing a skill. Git installations can be refreshed with
`pi update git:github.com/johanvx/agentic` after new commits are published.

## Skills

| Skill | When to use it |
| --- | --- |
| [commit-writer](skills/commit-writer/SKILL.md) | Draft or review Conventional Commit messages; commit only on explicit request. |
| [docx](skills/docx/SKILL.md) | Read, create, or edit Word documents. |
| [pdf](skills/pdf/SKILL.md) | Inspect, create, or modify PDFs, forms, and scans. |
| [pptx](skills/pptx/SKILL.md) | Read, author, or edit PowerPoint decks. |
| [pr-writer](skills/pr-writer/SKILL.md) | Draft PR/MR descriptions without publishing a PR. |
| [skill-creator](skills/skill-creator/SKILL.md) | Create or review Pi skills. |
| [tmux](skills/tmux/SKILL.md) | Work with tmux panes, including authenticated SSH shells. |
| [uv](skills/uv/SKILL.md) | Manage new Python work with `uv` without changing an existing project's tooling. |
| [xlsx](skills/xlsx/SKILL.md) | Inspect or edit spreadsheets and formulas. |
