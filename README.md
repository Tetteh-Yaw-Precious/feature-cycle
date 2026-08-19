# feature-cycle-generic

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

## Install

Copy the `feature-cycle-generic/` folder into your agent's skills directory:

```bash
# Claude Code
cp -R feature-cycle-generic ~/.claude/skills/

# Codex / Gemini CLI (cross-runtime alias)
cp -R feature-cycle-generic ~/.agents/skills/
```

Or clone and symlink so you can `git pull` updates:

```bash
git clone https://github.com/Tetteh-Yaw-Precious/feature-cycle-generic.git
# the repo folder contains a skill folder of the same name — the doubled path is correct
ln -s "$(pwd)/feature-cycle-generic/feature-cycle-generic" ~/.claude/skills/feature-cycle-generic
```

## Set up a project

The first time you run the skill in a repo, it will prompt you to create a profile. Do it by hand:

```bash
cp feature-cycle-generic/profile-template.md .claude/feature-cycle-profile.md
# then fill in every field with your project's facts
```

The skill looks for `.claude/feature-cycle-profile.md` in the repo you're working in. If a field
is blank it asks rather than guessing.

See `feature-cycle-generic/profiles/example.md` for a fully filled-in profile (a fictional
project) so you know what each field should look like.

## Companion skills (optional)

The loop hands off to a few other skills when present — `feature-boilerplate` (epic + issues),
`mermaid` (diagram syntax), `artifact-diagramming` (whether a diagram earns its place), and
`code-review`. If you don't have them, the main loop still runs; it just can't delegate those
specific steps.

## License

MIT
