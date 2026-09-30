# VIS Scheduler v3 — Project Rules

Read this file at the start of every session and follow it without being asked.

## What this project is

VIS Scheduler is a crew-scheduling web app for **VIS Productions AB** (visproductions.se),
a Swedish live-entertainment production company. It replaces spreadsheets for syncing
shifts, availability and communication across departments and productions.

This is **v3**: a full rebuild from scratch, done because the previous version kept
producing recurring bugs. Do not copy code or patterns from older versions unless I ask.
v3 starts with a brand-new deployment and an **empty database**. No data is migrated.

I (the owner) personally maintain the live app. I am not a professional developer, so
explain things in plain language and keep the codebase simple enough to follow.

## Tech stack

- **Cloudflare Workers** for the app and API
- **Cloudflare D1** (SQLite) for the database, Western Europe region
- Live URL: `scheduler.lourenco-quentin.workers.dev`
- Deployment: this GitHub repo is connected to Cloudflare. Merging into `main` deploys.
  Never assume a change is live until it is merged.
- No custom domain for now. visproductions.se DNS is at one.com and mail runs on
  Google Workspace; using a subdomain would require moving DNS to Cloudflare, so don't
  set that up unless I ask.

## Naming

- Always say **"Production"**, never "Show". This applies everywhere: UI text, code,
  variable names, database tables and columns, Help docs.

## Features and decisions already settled

- **Sign-in:** email sign-in links (magic links) built into the app.
  Do NOT use Cloudflare Access (rejected on cost grounds).
- **In-app notifications** and **availability approvals** are part of the app.
- **Fortnox** (payroll/contracts, Fortnox Lön) integration is deferred until after v3
  is live. Don't build it yet, but don't design anything that would block it later
  (e.g. keep worked hours per person per shift easy to export).

## Domain model: shift templates

Shifts are built from reusable **Shift Templates**. Current examples:

| Template | Role  | Headcount | Window      |
|----------|-------|-----------|-------------|
| T1       | Clown | 15        | 15:00–22:00 |
| T2       | Clown | 15        | 12:00–23:00 |
| T3       | Clown | 10        | 12:00–20:00 |
| T4       | Demon | 14        | 15:00–22:00 |
| T5       | Demon | 16        | 12:00–23:00 |
| T6       | Demon | 10        | 12:00–20:00 |

Each template has:
- a role, headcount, shift window, total hours, and team leader(s) (e.g. 2 slots)
- numbered **positions**, each belonging to a **zone** (1–4)
- a **start letter** and optional **end letter** per position (e.g. A → D means the
  position swaps from A to D partway through the shift; blank end letter = no rotation;
  combined letters like "C-D" are allowed)

More roles (e.g. Aerial, Fire) will be added later, so roles must not be hard-coded.

## Standing rules for every change

1. **Version number in the title.** Every new version of the app shows its version
   number in the page title (e.g. "VIS Scheduler v3.0.4"). Bump it with every release.
2. **Demo data.** Whenever a section is added or changed, include sample/demo data for
   it so I can see it working immediately.
3. **Help docs stay current.** Update the in-app Help documentation with every release
   so it matches the current version: new, changed and removed features, workflows
   and permissions.
4. **Build non-destructively.** Never delete data or drop database tables/columns
   without asking me first. Database changes go in numbered migration files.
   There must always be a way to roll back.
5. **Answer my questions first.** If my message contains a question, answer it before
   writing any code. Never start coding while a question of mine is unanswered.
6. **Plan before big changes.** For anything larger than a small fix, show me a short
   plan in plain language and wait for my OK.
7. **Keep this file up to date.** When I correct you, state a preference, or we settle
   a decision, add it to this file in the same change.

## Changelog

- v3.0.0 — Fresh start. (Update this list with each release.)
