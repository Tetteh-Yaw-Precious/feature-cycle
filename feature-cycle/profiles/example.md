# Feature-cycle profile — EXAMPLE (fictional)

A filled-in example so you can see what a real profile looks like. This describes a made-up
SaaS project called **Acme** — replace every value with your own. To set up your project,
copy `../profile-template.md` to `.claude/feature-cycle-profile.md` in your repo, not this file.

---

## Stakeholder
- **Who decides "done":** Dana, the product lead.
- **How to speak to them:** plain language, no jargon. "Shipped" means live in production and demoable, not just merged. Send a short "what changed" note after each release.

## Issue tracker
- **Tool:** GitHub Issues.
- **Command or URL:** `gh` CLI on `acme-co/acme-app`.

## Repos
- **acme-co/acme-web** — React frontend (Vite).
- **acme-co/acme-api** — Node/Express backend, Postgres.

## Base branch per repo
- **acme-web:** `develop`
- **acme-api:** `main`
- **Exceptions:** when extending an unmerged `feat/*` branch, branch from it and flag that the parent is still open.

## Branch conventions
- **Prefixes in use:** `feat/`, `fix/`, `chore/`, `docs/`.
- **Naming rule:** lead with the issue number, name for the outcome — `feat/214-export-invoices`.

## Worktrees
- **Worktree dir:** `.worktrees/` (add it to `.gitignore`).
- **Env files to copy into a fresh worktree:** `.env`, `.env.local`.
- **Dep install per worktree:** `npm install` (~1 min).

## Personas & role gotchas
- **Roles:** admin, member, viewer.
- **Gotchas:** a "workspace owner" is an admin with billing rights — check which is meant before gating a feature on it.

## Platform constraints
- **Mobile:** responsive web required; no native app.
- **Other:** none.

## Code conventions
- **Where copy/strings live:** `src/i18n/en.json`, not inline in components.
- **Casing rules:** UI button/label text in Title Case.
- **Other house rules:** shared UI comes from `@acme/ui`; do not hand-roll components that already exist there.

## Verification stack
- **Repo green?** Yes — `npm run typecheck` and `npm test` exit clean; trust the exit code.
- **Test baseline:** no known pre-existing failures.
- **DB clone command:** `createdb acme_demo --template acme_dev`.
- **Dev ports:** web `5173`, api `3000`, Postgres `5432`.
- **Browser tool:** Playwright.
- **Seed / test account:** `demo@acme.test` / password `demo1234` (seeded by `npm run seed`).
- **Feature flags:** default off; enable in `.env.local` with `FLAG_<NAME>=true` before a demo.

## Long-lived / epic branches
- None currently. If you start one, give it a landing cadence and name who runs its migrations.
