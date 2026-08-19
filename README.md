# feature-cycle

> **In plain terms:** When someone builds a new piece of software with help from an AI assistant,
> the work often slips through the cracks — it gets finished but never saved, done but never shared,
> or left half-broken and forgotten. This is a checklist the AI follows so that doesn't happen. It
> makes sure every piece of work gets thought through, actually finished, saved, tested, and handed
> off before moving on. Think of the routine a good contractor follows so a renovation gets finished
> and signed off, instead of a half-painted room nobody comes back to — this is that, for building
> software.

The rest of this page is written for the developers who'll install it.

---

A repeatable process for shipping a feature, packaged as an [Agent Skill](https://agentskills.io)
for AI coding agents (Claude Code, Codex, Gemini CLI). A "skill" is a set of instructions your
agent loads on its own when the work matches — you don't run a command, you just work normally and
the agent follows the process.

**What it's for:** on real projects, the expensive failures aren't bad planning — they're work that
goes invisible. Finished but never committed. Merged but never announced. Broken in a branch nobody
ran. This skill is the loop that stops that: talk it out → question it → audit what already exists →
track it on your issue tracker → build in isolation → verify against a proven baseline → prove it
runs → document what's true → close the loop.

**How you use it:** once installed, just start feature work the way you already do — "let's build
X", paste a meeting note, or "what's next." Your agent recognizes it and walks the loop, asking you
questions along the way. There's no command to memorize.

**How it stays portable:** the skill carries the loop and the discipline; every project-specific
fact (stakeholder, repos, base branch, seed accounts, database, dev ports, test tooling) lives in a
small per-project **profile** you fill in once. Same loop, any project.

## Requirements

- An AI coding agent that supports skills (Claude Code, Codex, or Gemini CLI).
- `git` on your machine.
- The [`gh`](https://cli.github.com) GitHub CLI, if you want the issue-tracking steps to run against
  GitHub. Any other tracker works too — you just note it in your profile.

## What's in this repo

Two skills that work together:

- **`feature-cycle`** — the main loop. This is the one that drives everything.
- **`feature-boilerplate`** — scopes a large feature into a GitHub epic + user-story issues. The
  `feature-cycle` loop hands off to it at step 4 for big work. Install both; they ship together.

## Install

Copy both skill folders into your agent's skills directory:

```bash
# Claude Code
cp -R feature-cycle feature-boilerplate ~/.claude/skills/

# Codex / Gemini CLI (cross-runtime alias)
cp -R feature-cycle feature-boilerplate ~/.agents/skills/
```

Or clone and symlink both so you can `git pull` updates:

```bash
git clone https://github.com/Tetteh-Yaw-Precious/feature-cycle.git
# the repo folder holds two skill folders; symlink each into your skills dir
ln -s "$(pwd)/feature-cycle/feature-cycle"        ~/.claude/skills/feature-cycle
ln -s "$(pwd)/feature-cycle/feature-boilerplate"  ~/.claude/skills/feature-boilerplate
```

## Set up a project

The first time you run the skill in a repo, it will prompt you to create a profile. Do it by hand:

```bash
cp feature-cycle/profile-template.md .claude/feature-cycle-profile.md
# then fill in every field with your project's facts
```

The skill looks for `.claude/feature-cycle-profile.md` in the repo you're working in. If a field
is blank it asks rather than guessing.

See `feature-cycle/profiles/example.md` for a fully filled-in profile (a fictional project) so you
know what each field should look like.

## Companion skills

### Bundled — installs with this repo

- **[`feature-boilerplate`](feature-boilerplate/SKILL.md)** — used at **step 4** to turn a large
  feature (one that needs a migration, or spans multiple repos) into a tracked milestone → epic →
  user-story issues written in plain, user-facing language. It's included in this repo, so if you
  installed both folders above there's nothing extra to do. For small or medium work the loop just
  opens a single issue and never calls it.

### Optional — external, not included

At three other steps the loop can hand work to a skill you may already have. If you don't, it does
that step itself with a built-in fallback, so nothing breaks — you don't need any of these to start.

| Skill | What it is | Which step uses it | If you don't have it |
|-------|-----------|--------------------|----------------------|
| `code-review` | A structured code-review pass over your diff. [Claude Code ships one](https://docs.claude.com/en/docs/claude-code) as `/code-review`; other agents have their own. | **Step 8 (Review)** — after you've re-read your own diff. | The loop does a deliberate manual review pass instead. |
| `mermaid` | Helper for writing correct [Mermaid](https://mermaid.js.org) diagram syntax. | **Step 10 (Document)** — when producing the architecture doc. | The loop writes the Mermaid diagrams itself. |
| `artifact-diagramming` | Judgment on whether a given diagram earns its place. A personal skill, no public home. | **Step 10 (Document)** — deciding which diagrams to keep. | The loop cuts any diagram that doesn't answer a real question, using its own judgment. |

So the loop runs fully standalone. The bundled `feature-boilerplate` covers the one heavy step;
the three optional skills just let it delegate those moments to a purpose-built skill instead of
handling them inline.

## License

MIT
