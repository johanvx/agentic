# Skill design guidance for Pi

Use these conventions as defaults, not a mandatory outline for every skill. The skill's domain and the user's requirements determine which sections and resources are useful.

## Format and discovery

- Use a directory named for the skill with a `SKILL.md` at its root. Match the frontmatter `name` to the directory name for portability. Names use lowercase letters, numbers, and single hyphens, with no leading or trailing hyphen, and at most 64 characters.
- Include `name` and `description` in YAML frontmatter. The description should say what the skill does **and** when an agent should use it; keep it under 1024 characters. Missing descriptions prevent Pi from loading a skill. Consult the installed Pi Skills documentation for optional fields and version-specific behavior.
- Keep the main file focused on instructions needed once the skill loads. Pi advertises the name, description, and path first; the model reads the full instructions when needed.
- Put longer guidance in `references/`, reusable forms in `templates/`, runnable code in `scripts/`, and output materials in `assets/` as needed. Point to each supporting file from `SKILL.md` with a relative path and an indication of when to read or use it. Include all referenced files in the delivered skill; copying only `SKILL.md` can break its links.
- Place skills meant for a Pi Git package under the package root's `skills/<name>/`. Other resource types have their own conventional directories; do not assume a skill bundles agent-level settings, credentials, or system prompts.
- For new skills that use Python, prefer `uv` in their scripts and examples, make that requirement clear, and tell users to stop and choose a fallback if `uv` is unavailable. Respect existing projects' Python toolchains instead of silently migrating them.

## Writing and change management

- State the goal and the main sequence of decisions before implementation details. Explain why a non-obvious step matters. Prefer specific steps and examples over a rigid, universal section order.
- Ask questions only when answers change the result. Take clues from the user's request and files first. State provisional assumptions when proceeding with low-risk unknowns, and pause for decisions that would change the design.
- Preserve useful safeguards from the original skill, particularly honesty about evidence and avoiding unwanted overwrites. Add domain-specific protections where relevant rather than pasting the same safety policy into every skill.
- Make output formats explicit only when downstream use calls for a strict format. A review can use passes, issues, and recommendations; an ordinary creation request need not produce six ceremonial sections or mandatory integrity tags. Flag unsupported claims plainly when they arise.
- When revising, identify intended behavior and examples before editing. Do not erase a user's scripts, references, or conventions just because another skill uses a different layout.

## Evaluation without ceremony

Try a small set of realistic requests, including different valid uses and, when relevant, one tempting false trigger. Show the proposed cases to the user. For an existing skill, compare the same requests with its earlier version if practical. Inspect actual outputs and ask for feedback; use objective checks where possible and avoid reporting made-up scores. Do not run paid model evaluations without agreement. Defer automated benchmarking until its added cost or complexity is justified.

For model-based runs, read [testing a skill in Pi](testing.md). It separates automatic selection from forced invocation, controls competing resources, and explains what CLI events do and do not establish. Keep the Pi version, provider/model, tools, settings, and fixtures consistent when comparing revisions.

Anthropic's [skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) inspired this lightweight draft–test–feedback cycle. Its Claude-specific commands, evaluation scripts, viewer, and `.skill` packaging are not bundled here; use Pi facilities instead when appropriate.
