---
name: feature-cycle-generic
description: |
  The portable end-to-end loop for building a feature on any project: talk it out → question it → audit what already exists → track it on the issue tracker → build in isolation → verify against a proven baseline → prove it runs → document what is true → close the loop. Triggers whenever the user starts new feature work — "let's build X", "start on this feature", "here's what the client asked for", "work through the meeting actionables", "what's next", pastes a meeting transcript, or asks to pick up where the last cycle ended. Reads a per-project profile for the concrete facts (stakeholder, repos, seed accounts, DB, ports, branch base). If a project-specific feature-cycle skill already exists for this repo, prefer it. Optionally calls the feature-boilerplate, mermaid, artifact-diagramming and code-review skills if they are installed, rather than repeating them.
---

# Feature Cycle (portable)

The loop for building a feature and never letting it go invisible. Each pass ends by surfacing what the next one starts from.

This skill exists because the expensive failures on real projects are not scoping failures. Work gets built and then disappears — finished but uncommitted, merged but never announced, broken in a branch nobody ran. Step 0 and step 3 are the ones that fix that. Everything else is chaining tools that already exist.

The skill carries the loop and the discipline; every project-specific fact lives in a per-project **profile** (see step 0a). Fill the profile in once for your project and the same loop runs correctly there.

## 0a. Load the project profile — do this before anything else

Every concrete fact this skill needs — who the stakeholder is, the repos, the base branch, the seed accounts, the DB clone command, the dev ports, the verification stack — lives in a **profile**, not here. That is what makes this reusable instead of wrong on every project but one.

**Find it, in this order:**

1. `.claude/feature-cycle-profile.md` in the repo you are working in (preferred — it travels with the project).
2. A profile the user names or pastes.
3. None yet? Copy `profile-template.md` (next to this SKILL.md), fill it in *with the user*, and write it to `.claude/feature-cycle-profile.md`. Do not guess the values — a wrong seed account or base branch is worse than asking.

Everywhere below that says **[profile: X]**, read X from the loaded profile. If the profile is silent on something, ask; do not fall back to a default carried over from another project.

## The fast path

**No time? Do these four and stop:**

1. **Close out what is loose** — commit anything finished, land anything ready
2. **Audit** — check it does not already exist
3. **Build it**
4. **Say what shipped** — to the stakeholder **[profile: stakeholder]**, in their language

Everything below is the full version. It is worth running when there is room, but a process that only works with an hour to spare is one you abandon on the day it would have helped most. Four steps in thirty minutes beats eleven steps skipped.

## Non-negotiables

Three rules hold no matter how small the work is, or how little time there is:

1. **Audit before you build.** Never write code for something until you have checked whether it already exists.
2. **Every piece of work gets a tracker issue.** The issue tracker **[profile: issue tracker]** is the project management tool. Untracked work is invisible work.
3. **Close the loop.** Committed, landed, issue closed, and the stakeholder told. "Done" on your machine is not done.

---

## 0. Close out the last cycle — do this first

**Before any new work.** Not at the end of a session, when you are tired and something more interesting is always available. At the start, when there is no momentum to protect.

```bash
git status --short                    # anything finished but uncommitted?
git worktree list                     # branches held, work parked mid-flight
gh pr list                            # anything sitting in review?
gh issue list --state open            # anything describing work already shipped?
```

Then:

- **Commit anything finished.** Work that exists only in a working tree is one `git checkout` from gone. This is not hypothetical — on a real project a completed nav fix sat uncommitted through an entire client demo and was invisible to every branch built from.
- **Land what is ready**, or say plainly why it is not moving. A branch open two weeks is a problem, not a state.
- **Close issues describing shipped work.**
- **Write the "tell the stakeholder" list** — what shipped since you last spoke, in their language **[profile: stakeholder]**. Shipped work they do not know about counts for nothing.
- **Name what is still open** and why.

Whatever this surfaces is the input to step 1. If the previous cycle ended cleanly, this takes a minute and confirms it.

---

## 1. Get it out of their head

The user talks the feature through in plain language — the messy version, edge cases, the "oh and also" parts. Do not interrupt with structure. Let the whole thing land first.

Two possible inputs:

- **They talk it out.** Take it as-is, no reshaping.
- **A meeting transcript.** Then the rule is the stakeholder's *exact words*, never a summary. Summaries lose the thing that matters — a single throwaway line often turns out to be the whole requirement. When something is ambiguous, quote them and ask, rather than resolving it yourself.

## 2. Question it

Now interrogate. Aim at the parts that change what gets built:

- Who is this for? Check the personas and role gotchas **[profile: personas & role gotchas]** — projects often have a role that is secretly two things, or a label that means something non-obvious.
- What happens on the unhappy path — nothing found, permission denied, half-filled form?
- Does this need to work on mobile? Honor the platform constraints **[profile: platform constraints]** (e.g. mobile input patterns).
- Does it touch money, contracts or personal data? Those raise the bar on review.
- What does "done" look like from the stakeholder's side — what would they click to believe it?

Tighten until you could build it without guessing. Then say back what you understood, in their words, and get a yes.

## 3. Audit — the step that saves the most time

**Before writing anything.** Check whether it exists already, in this order:

```bash
git log --all --oneline -15 -i --grep="<topic>"     # has this been built?
git branch -a --contains <sha>                       # is it merged, or stuck in a branch?
gh pr list --limit 20                                # is it sitting in review?
gh issue list --search "<topic>" --state all         # is there already an issue?
git status --short                                   # is it finished but uncommitted?
```

Then grep the code for the actual surface. Verify by reading, not by memory.

Classify every item as one of:

| Verdict | Meaning |
|---|---|
| **Shipped** | On production. Do not rebuild — tell the stakeholder instead. |
| **In a branch** | Built, unmerged. The work is landing it, not writing it. |
| **Uncommitted** | Finished but in nobody's branch. Commit it first — it is one `git checkout` from gone. |
| **Partial** | Some exists. Find the seam; the job is usually smaller than it looks. |
| **Not started** | Genuinely new. |

Report the verdicts before proposing work. On a real backlog this step routinely finds items already done and turns a "big build" into a missing link between two things that already existed.

## 4. Track it on the issue tracker

Every item gets an issue on **[profile: issue tracker]**. Scale only the ceremony:

- **Small or medium** — one issue, written straight from step 2, in the user's language rather than implementation terms. No epic.
- **Large** — needs a migration, or spans multiple repos, or is more than a couple of days: if the **`feature-boilerplate`** skill is installed, invoke it for the full milestone → epic → user-story treatment. If not, do it by hand: open a milestone, an epic issue, and one user-story issue per slice.

The size trigger is concrete: **does it need a migration, or more than one repo?** If no to both, one issue is enough.

Repos: **[profile: repos]**.

## 5. Branch and isolate

**Always fetch first, and branch from the remote — never from a local base that may be stale.**

```bash
git fetch origin
git worktree add -b <prefix>/<name> <worktree-dir>/<short-name> origin/<base>
```

Worktree directory and base branch come from **[profile: worktree dir]** and **[profile: base branch per repo]**. One worktree per concern, so unrelated work never tangles and the user's working tree is untouched. **The user's working tree is off limits** — never commit or stash in it without being asked.

### Naming

Use the branch prefixes the repo already favors **[profile: branch prefixes]**. Typical set:

| Prefix | For |
|---|---|
| `feat/` | new functionality |
| `fix/` | something broken |
| `chore/` | maintenance, tooling, deps |
| `copy/` | wording only |
| `refactor/` | structure, no behaviour change |

Match the dominant convention rather than inventing drift — if history uses `feat/` and not `feature/`, use `feat/`.

Name for the outcome, not the mechanism: `feat/owner-marks-job-complete`, not `feat/lead-patch`. Since every piece of work has an issue, leading with its number ties the branch, PR and issue into one chain — `feat/195-verified-badges`.

**The worktree takes the branch's slug, without the prefix or issue number** (`feat/195-verified-badges` → `<worktree-dir>/verified-badges`). `git worktree list` should read as a table of contents. **No abbreviations** — write the words out; the path costs nothing. **Never reuse a name for different work.** **When a feature spans multiple repos, use the same worktree name in each.**

### Choosing the base

**Default: the profile's base branch per repo [profile: base branch per repo].** That is the rule.

The one exception: when you are extending code that has not landed yet, branch from *that* branch or you are building against code that does not exist. **Treat needing that exception as a warning, not a normal case** — it means something has been unmerged long enough to become a base of its own. When you hit it, say so, and ask whether landing the parent first is the better move.

### Worktrees, and running several agents at once

**Check what exists before creating anything:** `git worktree list`, then `git worktree prune` for entries whose directory is gone.

**One branch, one worktree — git enforces this.** If a branch you want is already held, branch *from* it (`origin/<branch>`) instead of checking it out.

**Always create inside the profile's worktree dir**, which the repo ignores. Avoid job temp directories — they outlive the session, keep holding their branch, and are invisible until someone runs `git worktree list`.

**A worktree isolates code and nothing else.** Ports, database and containers are shared **[profile: dev ports, DB]**:

- **One app instance at a time**, or give each agent explicit ports.
- **Never let two agents migrate the same database.** Give each its own via the clone command **[profile: DB clone command]**.
- **Env files do not come with a worktree** — they are gitignored, so a fresh worktree has none and will point at the wrong API or fail to boot. Copy them in **[profile: env files]**.
- **`node_modules`/deps are per worktree.** Budget for the install; do not assume a new worktree is ready to run.

**Give parallel agents non-overlapping surfaces.** Two agents editing the same config on separate branches will not conflict while they work and will conflict at merge. Split by feature area, and say which files each owns.

Do not tidy up worktrees or branches after merging unless asked — iteration continues in them.

### Long-lived branches for big features

For something large — new tables, multiple repos, weeks of work — a feature branch that smaller branches merge into is reasonable. The cost is real: long-open epic branches drift from base and hide migration chains that cannot run. If you use one, it needs a landing cadence and someone actually running its migrations. Prefer merging in slices over accumulating one large branch.

## 6. Build

Match the surrounding code — comment density, naming, existing patterns. Prefer extending a proven pattern over inventing one; if there is already a signal, a guard, a helper doing something similar, mirror it.

Honor the project's code-layout conventions **[profile: code conventions]** (where copy lives, casing rules, etc.).

## 7. Verify against a proven baseline

Never trust an exit code or a total if the repo is not green **[profile: verification stack]**.

- **Type-check / lint**: capture output, then check **per file you touched**. Totals fluctuate if the repo generates code.
- **Tests**: if the suite has pre-existing failures, prove it — `git stash`, run, compare, `git stash pop`. Do not assume.
- **Migrations**: apply against a throwaway or cloned database **[profile: DB clone command]**, never the user's dev DB. Test the reverse too.
- **Behaviour**: for browser-facing work use the project's browser tool **[profile: browser tool + seed accounts]**. Auth against the local backend with the seed accounts from the profile.

State what you verified and what you did not. "Type-checks clean" and "I ran it" are different claims.

## 8. Review your own work

Before showing anything, re-read the diff looking for what you got wrong — not what you wrote. This reliably finds things: a missing import, an N+1 from a serializer field, a state you did not consider.

Then, if the **`code-review`** skill is installed, invoke it for the real pass. If not, do a deliberate review pass yourself: read the whole diff once more against the requirements, checking edge cases, error paths, and anything that touches money, contracts, or personal data.

## 9. Prove it runs

For anything the stakeholder will demo, a passing test is not proof. Stand it up:

- Clone the dev DB rather than migrating it **[profile: DB clone command]**.
- Apply migrations to the clone, then seed whatever the feature needs to be visible. A feature gated on data nobody has shows a blank page.
- **Turn the feature flags on.** Anything built behind a flag defaults to off, which means the demo shows nothing.
- Verify through the API before handing over a URL.

## 10. Document what is true

For a feature worth explaining to the stakeholder, produce the architecture doc:

- Verify every claim against code first — real enum values, real table names, real counts. This is the majority of the work; the diagrams are mechanical once you know what to say.
- Pick views by the question they answer, and cut any you cannot justify:

| Reader's question | Diagram |
|---|---|
| Who does what, in what order? | Flowchart |
| What does the system actually do? | Sequence |
| What can this thing *be*? | State — only when it has independent axes |
| What gets stored? | ER |

- Colour-code by **real ship state** — live / in a branch / not built — derived from git, never memory.
- Include a plain-language section on what is **not** built.

If the **`mermaid`** and **`artifact-diagramming`** skills are installed, use them — `mermaid` for diagram syntax, `artifact-diagramming` for whether a picture earns its place. If not, write the diagrams in plain Mermaid yourself and cut any that do not answer a real question. A stale doc is worse than none. Regenerate before each stakeholder meeting.

## 11. Close it — or let step 0 catch it

Ideally you close here: commit everything, open the PR referencing its issue, and write the "tell the stakeholder" list while the work is fresh.

Realistically this is the step that gets dropped — you are at the end of a session and closing out feels like admin. **That is exactly why step 0 exists and runs first.** Treat this as the preferred path, not the only one. What must not happen is both being skipped — that is how a finished fix ends up invisible through a demo, and how a PR sits unmerged for weeks hiding a migration chain that cannot run.

Whatever is left open — unlanded branches, unanswered questions, deferred scope — is the input to the next cycle's step 0.
