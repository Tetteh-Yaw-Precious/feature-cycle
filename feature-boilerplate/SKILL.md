---
name: feature-boilerplate
description: |
  Turn a feature idea into a fully-scoped design spec plus a GitHub epic with layman-friendly user-story issues. Triggers whenever the user says things like "let's scope a feature", "boilerplate this idea", "scope and push to GitHub", "build an epic for X", "I want to plan a feature", "create the epic and issues", "feature-boilerplate", or describes a new piece of functionality they want broken down into a backlog. Use this skill any time the user is about to start scoping new functionality and wants the work captured as an epic + user stories on GitHub, even when they don't name the skill explicitly. Walks the user through brain dump → clarifying questions → OOUX object model → gap-finding → v1 scope → committed design spec → GitHub milestone + epic + user-story issues with size and priority labels.
---

# Feature Boilerplate

A scaffolded conversation that turns a half-formed idea into a shippable backlog. The user provides a rough description; this skill drives them through OOUX-style scoping and lands a committed design spec plus a GitHub epic with user-story issues that read like a real user wrote them — not a developer.

## When you reach the end

The terminal artifact is **a GitHub milestone + epic + user-story issues**, plus a committed design spec in the repo. Do not start implementing. The next conversation can pick up the issues and build.

## The non-negotiables

- **One question at a time.** The user is thinking out loud; don't drown them in batched questions.
- **Brief is good.** The user wants momentum. Default to multiple choice when you can. Don't over-explain.
- **Layman-friendly stories.** User-story titles and acceptance criteria read like a non-technical person wrote them, from the persona's point of view. No JSON, no class names, no API jargon in issue bodies. Implementation detail belongs in the spec, not the issues.
- **Be opinionated.** When the user asks "what's best?" or "what do you think?", pick a position with reasoning. Don't punt.
- **OOUX first.** Objects, Relationships, CTAs, Attributes are the spine. UI follows from the object model.

## The flow

There are nine phases. Track them as todos so you don't lose the thread. Move through them with the user — do not skip ahead, but do not slow down for ceremony either.

### Phase 1 — Brain dump

Open with a single ask: *"Give me the upfront description — what are you trying to build, who's it for, and anything you already know about constraints or shape. Don't worry about structure, just dump it."*

Listen. Capture the obvious objects, personas, and constraints in your head. Don't ask follow-ups yet.

### Phase 2 — Clarify (one question at a time)

Now ask follow-ups, one at a time, in the order that has the biggest leverage on downstream decisions. Prefer multiple choice. Common high-leverage questions:

- Who is the user/persona? (gives you the voice for user stories)
- Where in the product does this live? (routes, existing surfaces)
- What persists, and where? (DB, IndexedDB, server, none)
- Is there a permission/auth model, or is it free for everyone in this flow?
- What's the v1 vs. later cut — what's the smallest thing that's still useful?
- Any decisions already made that I shouldn't relitigate?

Stop asking when you have enough to draw the object model. Usually that's 3–6 questions.

### Phase 3 — OOUX object pass

Present the object model. Use this exact framing so the user can scan it:

```
## Objects (N)

**Core**
- **ObjectName** — one-line role description.

**Components (live inside Core)**
- **ObjectName** — role.

**Relationship / customization layer (if applicable)**
- **ObjectName** — role.

## Relationships

| From | To | Cardinality | Note |
|---|---|---|---|
| A | B | one → many | ordered |

## CTAs (per object)

**ObjectName (in context)**
- **Discover:** verb • verb • verb
- **Commit:** verb • verb
- **Edit:** verb • verb
- **Lifecycle:** verb • verb
- *v2: verb • verb*  (flag deferred CTAs explicitly)

## Attributes (the ones that matter)

**ObjectName** — attr, attr, attr (note any "seeded for v2" attrs)
```

Then ask: *"Anything to add, cut, or rename?"*

### Phase 4 — Gap-finding pass

Proactively surface what's likely missing. Don't be exhaustive — be opinionated about the gaps that *actually matter*. The senior-engineer move is to call out things the user didn't think to ask about. Cover at minimum:

- **Empty states & first-visit onboarding** — what does day one look like?
- **Localization / language** — relevant in any global product; especially if the user mentioned a non-English market
- **Accessibility / mode variants** — e.g., "preview as patient" vs. "preview as clinician"
- **Versioning / migration** — what happens when the source data updates?
- **Cross-cutting axes** — tags / filters that cut across the primary grouping
- **The "we'll need this in v2 but should model the data now" trap** — flag attributes/objects to seed now even if no UI ships

Group your output into three tiers:
- ✅ Add to v1 — what you recommend folding in
- 🟡 Seed for v2 — model the data now, build the UI later
- ❌ Explicitly skip — name it so the user knows you considered it

End with: *"Want me to fold these in?"*

### Phase 5 — Lock v1 scope

Write a compact in/out list:

```
## v1 — in scope
- ...

## Explicitly out of v1 (parking lot)
- ...
```

Confirm with the user before continuing. *Don't write the spec yet.*

### Phase 6 — Write the design spec

Detect where specs live in this repo (in order):
1. `docs/superpowers/specs/` if it exists
2. `docs/specs/` if it exists
3. `docs/design/` if it exists
4. Ask the user where to put it

Filename: `YYYY-MM-DD-<kebab-topic>-design.md` using today's date.

Use this structure (adjust section depth to the feature's complexity — don't pad):

```markdown
# <Feature Name> — Design Spec

**Date:** YYYY-MM-DD
**Status:** Approved for issue breakdown
**Scope:** v1
**Source material:** <if any — e.g., a doc the user provided>

## Purpose
One paragraph. What this feature is and is not. Name what it's *not* (e.g., "this is the librarian, not the proctor").

## Guiding principles
Numbered list. The decisions that govern everything downstream.

## Object model (ORCA)
### Objects
Table or list.
### Relationships
Table.
### Key attributes
Per-object bullets. Mark attrs "seeded for v2" inline.

## CTAs
Grouped per object, grouped by intent (discover / commit / edit / lifecycle). Mark v2 CTAs explicitly.

## Editor / implementation choices
Only the *choices*, not the implementation. E.g., "block-based builder, not rich-text — because…"

## First-visit experience
What does onboarding look like?

## v1 scope — in / out
### In scope
### Out of v1 (seed in data model where noted)

## Technical envelope (high-level only)
Route, persistence, no-API caveat, etc. Implementation detail belongs in the implementation plan, not here.

## Open questions to revisit before implementation
Things flagged for the next conversation.

## Out of scope explicitly
Things considered and rejected.
```

After writing, do a 4-point self-review:
1. Any TBD/TODO/placeholder text? Fix.
2. Any internal contradictions? Fix.
3. Is the scope a single epic, or does it need decomposition?
4. Any ambiguity that could be read two ways? Pick one, make it explicit.

Fix issues inline. Commit the spec with a message that captures the decision (not just "add spec").

Then ask the user to review the spec file before continuing. If they request changes, make them and re-commit.

### Phase 7 — Prep GitHub

Detect repo and conventions:

```bash
gh repo view --json nameWithOwner,defaultBranchRef
gh label list --limit 100
gh api "repos/<owner>/<repo>/milestones?state=open" --jq '.[] | {number, title}'
```

Check for these labels (create if missing, only with user permission):
- `epic`, `user-story`
- Priorities — common patterns: `P0`/`P1`/`P2`/`P3`, or `priority: high/medium/low`
- Sizes — common patterns: `size: S`/`size: M`/`size: L`/`size: XL`
- Area labels — `frontend`, `backend`, `ui/ux`, `infra`, etc.

If the repo uses a different label scheme, adapt — match the repo's existing convention rather than imposing one.

### Phase 8 — Create milestone + epic + issues

Show the user the proposed structure *before* mass-creating issues — they should sign off on the story list, sizes, and priorities first. Format:

```
**Milestone:** <Area> — Epic: <Feature>
- Goal: ...
- Done when: ...

**Epic issue** — body links to spec, lists user stories grouped by tier.

**N user-story issues** — proposed:

| # | As a <persona>, I want to… | Priority | Size |
|---|---|---|---|
| 1 | ... | P1 | M |
```

After sign-off, execute in this order:

1. **Milestone** — `gh api repos/<owner>/<repo>/milestones -X POST -f title="..." -f description="Goal: ...\n\nDone when: ...\n\nSpec: <path>"`. Capture the milestone number.
2. **Epic issue** — `gh issue create` with `epic` label, attached to milestone. Body: Goal, Done when, Guiding principles, Objects table, User stories placeholder, Out of scope, Spec link. Capture the issue number.
3. **User-story issues** — create in parallel (`gh issue create` calls in one Bash batch). Each issue must:
   - Have a title written from the persona's POV ("Browse the catalogue by specialty", not "Implement catalogue listing component")
   - Have a body that follows the **As / I want / So that** pattern
   - Have an **Acceptance criteria** checklist written non-technically ("I can search by name", not "search input dispatches an action")
   - Reference the epic by number (e.g., "Epic: #146")
   - Carry labels: `user-story` + priority + size + relevant area labels
4. **Update the epic body** with the full user-story checklist, grouped by tier (Foundations / Customization / Polish, or whatever tiers fit the feature). Use `gh issue edit <epic-number> --body "..."`.

### Phase 9 — Wrap

Show the user:
- Milestone URL
- Epic URL
- Compact table of all stories with priority + size + URL
- Suggested order of attack (which stories unblock which)

Then stop. Do not start implementation.

## Issue body templates

### Epic body

```markdown
## Goal
<one paragraph — what this feature is and is not>

## Done when
<one sentence — observable, end-to-end>

## Guiding principles
1. ...

## Objects (ORCA)
| Object | Role |
|---|---|
| **A** | ... |

## User stories
### Foundations (P1)
- [ ] #N — Title  · `size: X`
### Customization (P1)
- [ ] #N — Title  · `size: X`
### Polish (P2)
- [ ] #N — Title  · `size: X`

## Out of scope (v1)
- ...

## Spec
Full design: [<spec-path>](<repo-relative-link>)
```

### User-story body

```markdown
## User story

**As a** <persona>
**I want** <outcome in plain language>
**So that** <reason that matters to the persona>

## Acceptance criteria

- [ ] I can / I see / I get … (non-technical, observable)
- [ ] ...

## Notes

<optional: tie back to the spec, ORCA object, or a parent decision>

Epic: #<epic-number>
```

## Common traps and how to avoid them

- **Skipping the brain dump.** The first message from the user *is* the brain dump — don't ask clarifying questions until they've finished. If they only give one sentence, ask them to expand before drilling in.
- **Asking too many questions.** Three to six clarifying questions is plenty. After that, present the object model and let the user react to *something* concrete.
- **Letting issue bodies become technical.** If you find yourself writing "POST /api/…" or "create a new Redux slice" in an issue body, stop. That belongs in the implementation plan, not the user story.
- **Forgetting to lock decisions before writing the spec.** If the user said "what do you think is best?" at any point, write down the decision before moving to the next phase. Otherwise the spec ends up vague.
- **Creating issues before the user signs off on the story list.** Show the proposed table first, get a yes, then mass-create.
- **Wrong cross-references.** When stories reference each other (e.g., "see Fork story"), you don't have the issue numbers until after creation. Use placeholders, then patch with a final edit pass.
- **Over-styling the issue bodies.** Keep markdown light. No emojis unless the user uses them.

## Style notes

- The persona's voice matters. If the persona is a "solo practitioner," write "As a solo practitioner, I want to…". If it's a "supervisor," "shopper," "ops engineer" — use that. Don't generic-ify to "user".
- Acceptance criteria should be observable. Not "the system is performant" — instead, "the catalogue loads instantly without a spinner."
- Don't over-decompose. A feature might be 6 stories or 20 — let the scope shape the count, not a quota.
- If a story is XL, flag in its body that it'll likely need further decomposition during planning. Don't pre-decompose it inside the epic.
- The spec is the source of truth for *why*. The issues are the source of truth for *what's being built next*. Keep that boundary clean.
