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

### Naming convention

Name skills by their subject or purpose: an artifact, activity, format, or
tool—not an agent role. Use concise, lowercase, hyphen-separated names
such as `scientific-poster`, `commit-writing`, and `skill-development`;
avoid role suffixes such as `-writer` and `-creator`. Keep established
format and tool names such as `pdf`, `docx`, `tmux`, and `uv`. The directory
name and the `SKILL.md` frontmatter `name` must match.

### Available skills

| Skill | When to use it |
| --- | --- |
| [commit-writing](skills/commit-writing/SKILL.md) | Draft or review Conventional Commit messages; commit only on explicit request. |
| [docx](skills/docx/SKILL.md) | Read, create, or edit Word documents. |
| [pdf](skills/pdf/SKILL.md) | Inspect, create, or modify PDFs, forms, and scans. |
| [pptx](skills/pptx/SKILL.md) | Read, author, or edit PowerPoint decks. |
| [pr-writing](skills/pr-writing/SKILL.md) | Draft PR/MR descriptions without publishing a PR. |
| [scientific-poster](skills/scientific-poster/SKILL.md) | Plan, build, and review assertion-evidence posters in Typst or PowerPoint. |
| [scientific-presentation](skills/scientific-presentation/SKILL.md) | Plan, build, and review assertion-evidence talks in Typst, PowerPoint, or Beamer. |
| [scientific-writing](skills/scientific-writing/SKILL.md) | Draft, revise, and proofread emails, letters, reports, and papers in Alley's style. |
| [skill-development](skills/skill-development/SKILL.md) | Create or review Pi skills. |
| [tmux](skills/tmux/SKILL.md) | Work with tmux panes, including authenticated SSH shells. |
| [uv](skills/uv/SKILL.md) | Manage new Python work with `uv` without changing an existing project's tooling. |
| [xlsx](skills/xlsx/SKILL.md) | Inspect or edit spreadsheets and formulas. |
