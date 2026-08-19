# Feature-cycle profile — TEMPLATE

Copy this to `.claude/feature-cycle-profile.md` in the project repo and fill every field.
The `feature-cycle` skill reads this for all project-specific facts. If a field is
unknown, ask the user — do not guess. A wrong seed account or base branch is worse than a blank.

---

## Stakeholder
- **Who decides "done":** <name / role>
- **How to speak to them:** <plain-language vs technical; any copy rules; what "shipped" means to them>

## Issue tracker
- **Tool:** <GitHub / Linear / Jira / …>
- **Command or URL:** <e.g. `gh` CLI on org/repo>

## Repos
- **repo-a:** <org/name> — <role, e.g. web frontend>
- **repo-b:** <org/name> — <role, e.g. backend API>
- (add more as needed)

## Base branch per repo
- **repo-a:** <e.g. master>
- **repo-b:** <e.g. main>
- **Exceptions:** <any unlanded branch that is currently a real base, and why>

## Branch conventions
- **Prefixes in use:** <feat/ fix/ chore/ copy/ refactor/ — note the dominant spelling>
- **Naming rule:** <e.g. lead with issue number, outcome not mechanism>

## Worktrees
- **Worktree dir:** <e.g. `.claude/worktrees/` — must be gitignored by every repo>
- **Env files to copy into a fresh worktree:** <e.g. `.env`, `.env.local`>
- **Dep install per worktree:** <e.g. `pnpm install`, ~how long>

## Personas & role gotchas
- **Roles:** <owner / professional / admin / …>
- **Gotchas:** <any role that is secretly two things; any label that means something non-obvious>

## Platform constraints
- **Mobile:** <required? any input patterns, e.g. dropdowns must be bottom drawers>
- **Other:** <tablet, webview, offline, …>

## Code conventions
- **Where copy/strings live:** <e.g. `features/**` not `routes/**`>
- **Casing rules:** <e.g. UI labels Title Case>
- **Other house rules:** <…>

## Verification stack
- **Repo green?** <is `tsc`/lint/test exit code trustworthy, or must you diff per-file?>
- **Test baseline:** <known pre-existing failures; how to prove them>
- **DB clone command:** <e.g. `CREATE DATABASE <name> TEMPLATE "<template>";` — note case-sensitivity/quoting>
- **Dev ports:** <e.g. web 5173, backend 8087>
- **Browser tool:** <e.g. playwright-cli>
- **Seed / test accounts:** <emails + shared password, and which backend they auth against>
- **Feature flags:** <default off? how to turn on for a demo>

## Long-lived / epic branches
- <any current epic branch smaller branches merge into, its landing cadence, migration owner>
