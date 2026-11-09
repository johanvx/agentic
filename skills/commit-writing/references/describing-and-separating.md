# Describing and separating changes

This guide adapts two sections of the Linux kernel's [Submitting patches](https://docs.kernel.org/process/submitting-patches.html) ("Describe your changes" and "Separate your changes") to ordinary Git repositories. It adds writing techniques from technical and scientific writing: Michael Alley's *The Craft of Scientific Writing*, and George Gopen and Judith Swan's "The Science of Scientific Writing" (*American Scientist*, 1990). The advice is paraphrased; the examples are illustrative, not claims about any repository.

## Who reads a commit message

A commit message outlives the discussion around it. Its readers are:
- someone scanning `git log --oneline` for one change among hundreds, who sees only the subject;
- someone running `git blame` on a puzzling line, who needs to know why the line is the way it is;
- someone running `git bisect`, who lands on the commit and must judge whether it caused a regression;
- a reviewer now, who compares the description with the diff.

None of them has the chat, the ticket, or the pull request in front of them. Write so the message works alone.

## The subject: one claim

Treat the subject like the headline of a scientific slide: a short statement of what the commit does, not a topic label.

| Weak | Better | Why |
|---|---|---|
| `fix: bug` | `fix(cache): drop stale entries after an update` | Says which bug, and where |
| `refactor: update parser.py` | `refactor(parser): split header parsing from row parsing` | A file name is not a change |
| `feat: improvements` | `feat(export): write CSV with a header row` | "Improvements" fits any commit |
| `fix: fixed the issue where timeouts were too short and also renamed config` | two commits | "And" signals two changes |

The imperative mood reads as an instruction to the codebase: *If applied, this commit will* "drop stale entries after an update". In a series, each subject must differ from its neighbors.

## The body: problem, impact, solution

1. **Problem.** What was wrong or missing, and under what circumstances? Even a one-line fix has a reason. For a refactor, the problem may be a maintenance cost, such as duplicated logic that drifted apart.
2. **Impact.** Who or what is affected, and how visibly: a crash, wrong output, a slow path, a confusing API. Downstream maintainers decide whether to backport from this paragraph.
3. **Solution.** What the change does, in plain English and enough technical detail to check against the diff. Explain a non-obvious choice and, when it matters, the alternative you rejected.
4. **Costs and evidence.** Give numbers for claimed gains, with the workload and the baseline ("parsing a 40 MB log takes 2.1 s instead of 3.4 s"). Name what gets worse: memory for speed, complexity for generality. Without measurements, describe the expectation as an expectation.

Quote the key line of an error message or log when people will search for it, trimmed of timestamps, addresses, and other noise. Refer to another commit by at least 12 characters of its hash and its subject, so the reference survives rebases of other branches and can be read without looking it up: `commit 0123456789ab ("fix(cache): drop stale lookup entries")`. A link (an issue, a discussion) can add detail, but the message must still explain the change without it.

## Writing techniques

- **Known before new.** Begin a sentence or paragraph with what the reader already knows (the function, the symptom just described), and end it with the new information. Readers expect the stress position at the end of a sentence to carry the point.
- **One idea per paragraph.** Problem, then solution, each in its own paragraph. A paragraph that changes topic halfway is two paragraphs.
- **Specific words.** "Retry the request up to three times" says more than "improve robustness". Prefer concrete verbs (drop, split, cache, reject) over "handle", "update", "deal with".
- **Consistent terms.** Call a thing by the same name throughout. If the code calls it a "session", don't switch to "connection" for variety; a reader will assume two different things.
- **No filler.** Cut "This commit", "basically", "in order to", "it should be noted that". Cut hedges the evidence doesn't require, and keep the ones it does.
- **Numbers with units and a baseline.** "From 120 ms to 45 ms at p99 on the staging load test", not "much faster".
- **Lists only for parallel items.** Three independent effects can be a list; an argument stays prose.

## Separate your changes

**One logical change per commit.** A logical change is something a reviewer can understand, justify, and revert on its own. Split:
- a bug fix from a performance improvement;
- a refactor from the behavior change it prepares;
- an API change from the first code that uses it (two commits, in that order);
- formatting or whitespace from logic;
- a move or rename from edits to the moved code, so the diff shows the real changes.

**Keep one logical change together.** A rename across forty files is one commit. A test belongs with the change it tests; a generated file belongs with its source.

**Keep every step working.** Each commit should build and pass the tests, because `git bisect` may stop on any of them. Put preparatory work first. When a commit depends on an earlier one, say so in the later body.

**Don't over-split.** A commit that cannot be justified alone ("add a helper nobody calls yet" with no follow-up in the same series) is noise. Split until each commit has one reason to exist, no further.

### A split plan

When the candidate changes hold several logical changes, propose the commits before staging anything:

```text
1. refactor(cache): move key building into cache_key()
   src/cache.py (hunks 1-2), tests/test_cache.py
2. fix(cache): drop stale entries after an update
   src/cache.py (hunk 3), tests/test_cache.py (new test)
   Depends on 1.
3. docs: describe cache invalidation
   docs/cache.md
```

Each commit gets its own full message. If the user prefers one commit, write a message whose subject and body name every part honestly.

## Revision checklist

- Does the subject alone tell a `git log --oneline` reader what changed?
- Does the body say why, before how?
- Is every claim (a fix, a speedup, a test run) backed by the diff, a measurement, or the user?
- Would the message make sense to someone who never saw the discussion?
- Does the commit do one thing? Would anything in it be reverted separately?
- Is each commit in a series working on its own, and in an order that makes sense?
