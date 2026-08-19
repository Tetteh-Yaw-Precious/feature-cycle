# feature-cycle-generic

A portable [Agent Skill](https://agentskills.io) for building a feature end to end without
letting the work go invisible: talk it out → question it → audit what already exists → track it
→ build in isolation → verify against a proven baseline → prove it runs → document what is true →
close the loop.

The skill carries the **loop and the discipline**; every project-specific fact (stakeholder,
repos, base branch, seed accounts, database, dev ports, verification tooling) lives in a small
per-project **profile** the skill reads at startup. Fill the profile once per project and the same
battle-tested loop runs correctly everywhere.

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

## Companion skills (optional)

The loop hands off to a few other skills when present — `feature-boilerplate` (epic + issues),
`mermaid` (diagram syntax), `artifact-diagramming` (whether a diagram earns its place), and
`code-review`. If you don't have them, the main loop still runs; it just can't delegate those
specific steps.

## License

MIT
