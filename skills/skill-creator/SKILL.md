---
name: skill-creator
description: Create, review, and improve Pi skills. Use when drafting a SKILL.md, auditing a skill's structure or instructions, refining an existing skill, or checking whether a skill triggers and works on realistic tasks.
---

# Skill Creator

Help the user build skills that are useful when invoked, not merely valid on disk. Target Pi by default; prefer the portable Agent Skills directory layout when it meets the user's needs.

Read [skill design guidance](references/skill-design-guide.md) when checking format, structure, or portability. For a new skill, start with the [minimal template](templates/skill-template.md), then remove sections the task does not need. Check the installed Pi Skills and Packages documentation if a Pi-specific behavior is uncertain.

## Establish the task

First use what the conversation and files already tell you. Determine whether the user wants a new skill, a review, or an improvement; identify the intended users, triggering situations, desired outcome, and any existing scripts, references, and examples. For a revision, read the entire skill and the supporting files it actually depends on before proposing changes.

Ask only for information that affects the design. Group a few targeted questions rather than running a fixed intake questionnaire; if two approaches have a real trade-off, explain it and let the user choose before committing to one. Do not invent missing facts or silently fill important gaps with assumptions. A request to edit a named skill authorizes edits to that skill, not deletion of unrelated work or replacement of a different skill.

## Design or review

- **Create:** Confirm the destination and check for collisions. Draft a specific frontmatter `description` that states both what the skill does and when it should be loaded. Map the user's workflow into short, actionable instructions.
- **Review:** Check the frontmatter, trigger description, workflow, supporting-file paths, safety, and fitness for the user's actual tasks. Report what works, what does not, and concrete improvements; do not rewrite a skill when asked only to review it.
- **Improve:** Identify strengths worth keeping before removing anything. Compare alternatives against the user's goals, preserve intended behavior, and explain changes that alter the skill's scope or contract. Ask before destructive changes or unresolved design choices.

Use `SKILL.md` for routing and the core workflow; put detailed, optional material in referenced files, and include scripts or templates only when they save repeated work. Explain when to read a reference. Keep links relative to the *delivered* skill directory, not to a source or template directory. Do not require every generated skill to adopt this skill's sections or output format.

Keep claims and examples honest: do not invent references, measurements, test outcomes, or technical capabilities. Mark unverified details as such, or ask the user when they affect a decision. Inspect unfamiliar scripts before recommending or running them; never assume Claude-specific commands, subagents, or viewers are available in Pi.

## Check and improve with examples

1. Check the directory and `SKILL.md`: matching, valid name; useful description; readable instructions; resolvable bundled-file links; and dependencies or environment requirements stated where needed. For Pi-specific features, verify against the installed Pi documentation rather than guessing.
2. Propose two or three realistic prompts that exercise different parts of the skill. Include a near miss when deciding whether its description is too broad. For a revision, compare against the previous version where practical.
3. Ask for the user's view of the proposed prompts. If model runs are worthwhile and the user agrees to their cost, read [testing a skill in Pi](references/testing.md) for controlled resource selection, separate automatic/forced cases, and event-based verification. Record the test environment and observed behavior, not imagined runs or numbers. Static checks and a review of the draft are enough when live runs are not warranted.
4. Revise from the observations and the user's feedback, aiming for reusable guidance rather than rules tailored only to the example prompts. Repeat when there is a specific problem to address; automated benchmarks are optional, not a prerequisite.

## Deliver

For a review, provide passes, issues with locations and suggested fixes, and optional recommendations. For a new or revised skill, point to the files created or changed, explain consequential decisions, and state what was and was not tested. Surface remaining uncertainty and the next useful action without imposing a six-part report on every interaction. Do not install the skill or change Pi settings unless the user asks.
