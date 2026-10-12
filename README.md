# feature-cycle

> **In plain terms:** When someone builds a new piece of software with help from an AI assistant,
> the work often slips through the cracks. It gets finished but never saved, done but never shared,
> or left half-broken and forgotten. This is a checklist the AI follows so that doesn't happen. It
> makes sure every piece of work gets thought through, actually finished, saved, tested, and handed
> off before moving on. Think of the routine a good contractor follows so a renovation gets finished
> and signed off, instead of a half-painted room nobody comes back to. This is that, for building
> software.

The rest of this page explains what it is and exactly how it works, then how to install it. You
don't need to already know the tools to follow along.

## What am I looking at?

This is a **skill for AI coding agents** like Claude Code, Cursor, Codex, and Gemini CLI, the tools
that write code alongside you. A skill is just a set of written instructions the agent reads and
follows. You don't run it like an app or type a command for it. You install it once, then work the
way you normally would, and when you start building a new feature the agent notices and quietly
follows the process described here.

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
   similar), described in plain language. Big features get broken into a proper plan first, which is
   the job of the companion `feature-boilerplate` skill.
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

## It talks to you while it works

This is the part most AI coding sessions get wrong, so the skill is strict about it. The failure is
specific: the agent checks what already exists, you say "go", and then an hour disappears with no
word while branches and changes pile up. You cannot redirect what you cannot see, and by the time
you ask what's happening, the expensive decisions are already made.

So there are three moments where the agent stops and talks to you, every time: after it has checked
what already exists, after each finished piece of work, and whenever a decision has more than one
reasonable answer. Working ahead quietly is fine once a decision is made. It is never fine for the
decision itself.

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

- An AI coding agent that supports the Agent Skills standard: Claude Code, Cursor (2.4+), Codex,
  Gemini CLI, or GitHub Copilot CLI. They all read the same `SKILL.md` format, so one copy of this
  skill works in any of them.
- `git` on your machine.
- The [`gh`](https://cli.github.com) GitHub CLI, if you want the issue-tracking steps to run against
  GitHub. Any other tracker works too, you just note it in your profile.

## What's in this repo

- **`SKILL.md`** is the loop itself, the instructions your agent reads.
- **`profile-template.md`** is the blank form you fill in once per project.
- **`profiles/example.md`** is a completed profile for a made-up product, so you can see what a good
  answer to each field looks like.

## Install

Every agent reads the same `SKILL.md` format; only the folder it looks in differs. Find your agent
in the table, then run the matching clone:

| Agent | Where skills live |
|-------|-------------------|
| Claude Code | `~/.claude/skills/` (your home folder, used by every project) |
| Codex | `~/.agents/skills/` |
| Gemini CLI | `~/.agents/skills/` |
| GitHub Copilot CLI | `~/.agents/skills/` |
| Cursor (2.4+) | `.cursor/skills/` **inside the project** you want to use it in |

```bash
# Claude Code
git clone https://github.com/Tetteh-Yaw-Precious/feature-cycle.git \
  ~/.claude/skills/feature-cycle

# Codex, Gemini CLI, or Copilot CLI
git clone https://github.com/Tetteh-Yaw-Precious/feature-cycle.git \
  ~/.agents/skills/feature-cycle

# Cursor, from inside the project you want it in
git clone https://github.com/Tetteh-Yaw-Precious/feature-cycle.git \
  .cursor/skills/feature-cycle
```

That's it. Your agent will pick the skill up automatically the next time you start feature work.
Because it's a clone rather than a copy, `git pull` inside that folder gets you later changes.

## Set up a project

The first time you run the skill in a repo it will offer to create a profile with you. To do it by
hand instead:

```bash
cp ~/.claude/skills/feature-cycle/profile-template.md .claude/feature-cycle-profile.md
# then fill in every field with your project's facts
```

The skill looks for `.claude/feature-cycle-profile.md` in the repo you're working in. If a field is
blank it asks rather than guessing, because a wrong database or branch name is worse than no answer
at all.

See `profiles/example.md` for a fully filled-in profile so you know what each field should look
like.

## Companion skills

The loop hands work to these rather than repeating them. Each one is optional. If you don't have it
installed, the loop does a reasonable version of that step itself and carries on.

| Skill | When the loop uses it |
|---|---|
| [`feature-boilerplate`](https://github.com/Tetteh-Yaw-Precious/feature-boilerplate) | Step 5, for a feature big enough to need a real plan. Turns a rough idea into a tracked milestone, an epic, and user-story issues written in plain language. |
| [`vertical-slice-matrix`](https://github.com/Tetteh-Yaw-Precious/vertical-slice-matrix) | Steps 4 and 6, before writing any small reusable piece of code, and before adding a value to a fixed list like order statuses or payment methods. Searching the project's history tells you whether a *feature* exists. It cannot tell you that the same helper has already been written five times. |
| [`lifecycle-feature`](https://github.com/Tetteh-Yaw-Precious/lifecycle-feature) | Step 6, when the thing being built is shared between two people who both act on it, like an invitation or an approval. |
| [`architecture-guardrails`](https://github.com/Tetteh-Yaw-Precious/architecture-guardrails) | Step 8, when a bug keeps coming back and deserves an automatic check instead of a rule somebody has to remember. |
| [`feature-docs`](https://github.com/Tetteh-Yaw-Precious/feature-docs) | Steps 2 and 10, when the feature needs a written specification before building or a real document after. |

## License

MIT.
