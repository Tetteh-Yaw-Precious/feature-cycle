# feature-cycle

> **In plain terms:** When someone builds a new piece of software with help from an AI assistant,
> the work often slips through the cracks — it gets finished but never saved, done but never shared,
> or left half-broken and forgotten. This is a checklist the AI follows so that doesn't happen. It
> makes sure every piece of work gets thought through, actually finished, saved, tested, and handed
> off before moving on. Think of the routine a good contractor follows so a renovation gets finished
> and signed off, instead of a half-painted room nobody comes back to — this is that, for building
> software.

The rest of this page explains what it is and exactly how it works, then how to install it. You
don't need to already know the tools to follow along.

## What am I looking at?

This is a **skill for AI coding agents** like Claude Code, Codex, and Gemini CLI, the tools that
write code alongside you. A skill is just a set of written instructions the agent reads and follows.
You don't run it like an app or type a command for it. You install it once, then work the way you
normally would, and when you start building a new feature the agent notices and quietly follows the
process described here.

The process has a name: the **feature cycle**. It is a start-to-finish routine for building one
feature and making sure it actually ships. Thought through, built, tested, saved, and handed off,
instead of getting lost halfway.

## The problem it fixes

Most software doesn't fail because someone planned it badly. It fails in quieter ways:

- The work gets finished but never saved, so it vanishes the next time the code is reset.
- It gets merged into the main project but nobody is told, so the person who asked for it thinks it
  was never done.
- It sits in a side copy that looks finished but was never actually run, so it's broken and no one
  notices until the demo.

These failures are boring, and they are expensive. The feature cycle is built to make each one hard
to skip past.

## How it works

Once installed, you don't call it. You start feature work the way you already talk: "let's build a
booking screen", "here's what the client asked for", or you paste in notes from a meeting. The agent
recognizes that as the start of a feature and walks these steps with you, asking questions as it
goes:

1. **Clean up loose ends first.** Before anything new, it checks for work from last time that was
   finished but never saved or announced, and closes it out. This is where lost work gets caught.
2. **Get the idea out of your head.** You describe the feature in plain words, the messy version,
   edge cases and all. The agent listens without reshaping it.
3. **Question it.** It asks the things that actually change what gets built: who is this for, what
   happens when something goes wrong, does it need to work on mobile, does it touch money or personal
   data.
4. **Check it doesn't already exist.** Before writing a single line, it searches the project's
   history and code to see if the feature, or part of it, is already built. This step alone often
   turns a "big build" into a small one.
5. **Write it down as a task.** Every piece of work gets an issue on your tracker (GitHub or
   similar), described in plain language. Big features get broken into a proper plan first. That is
   the job of the bundled `feature-boilerplate` skill.
6. **Build it in a safe copy.** The work happens in an isolated branch so your main code is never
   touched until it's ready.
7. **Check it honestly.** It runs the type-checks and tests, and for anything users will see it
   actually opens the app in a browser to confirm the feature works, rather than trusting a green
   checkmark.
8. **Review its own work.** It re-reads everything it just wrote, looking for its own mistakes, then
   runs a proper code review.
9. **Prove it runs.** For anything you'll show someone, it stands the feature up with real data and
   confirms it works from end to end, so a demo never opens to a blank screen.
10. **Write down what's true.** For features worth explaining, it produces a short document, often
    with diagrams, describing how the thing actually works. Every claim is checked against the real
    code, not guessed.
11. **Close the loop.** It saves everything, opens the work for review, and writes a short "here is
    what shipped" note in plain language for whoever asked.

In a hurry? There's a built-in short version: clean up loose ends, check it doesn't already exist,
build it, and say what shipped. Four steps instead of eleven.

## How it fits any project

Every project is different: different people, code, databases, and test setups. Instead of baking
those details into the skill, the skill keeps them in a small separate file called a **profile**.
You fill it in once per project. Who to report to, which code repositories, how to run the tests,
where the database lives, and so on. The steps above stay the same everywhere; the profile is the
part that changes. This repo includes a blank template and a filled-in example to copy from.

## What a session looks like

> **You:** let's build a way for owners to mark a job complete
>
> **Agent:** First, let me check nothing from last session is unsaved. Clean. Now, who marks it
> complete, only the owner or staff too? And what should the worker see when it happens?
>
> **You:** just the owner. the worker gets a notification.
>
> **Agent:** Got it. Before I build, let me search the project. There's already a `job_status` field
> and a notification helper. This is smaller than it looks, so I'll wire those together instead of
> building new. Here's the task I'll track…

That back-and-forth is the whole point. It isn't a wizard you click through. It's the agent working
the way a careful teammate would.

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
