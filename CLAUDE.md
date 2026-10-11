# CLAUDE.md

Standing rules for this project. Read this and README.md at the start of
every session. README is the vision. This file is the law. When the two
conflict, this file wins, and flag the conflict, ask permission, therefore it gets fixed and all parties are aware.

## What this is

Life OS. A personal operating system for one person's life. A headless
backend first, a dashboard later. Tasks is the first module.

## Hard rules

These are not suggestions. Do not break them without asking first.

1. **Single user.** One person uses this. No accounts, no signup, no
   multi tenant logic, no user tables beyond what auth strictly needs. Do
   not build for scale or for other people.
2. **Headless first.** No UI work of any kind until explicitly told the
   headless phase is over. No frontend frameworks, no styling, no
   templates. The only interface is the API and the terminal readout
   script.
3. **Every AI call goes through the brain.** One module owns all LLM
   access. Nothing else imports a provider SDK or calls a model API
   directly. The brain takes text in and returns validated structured
   data out. Swapping providers must be a config change.
4. **Modules are self contained.** Each module lives in its own folder,
   owns its own data and routes, and registers itself with the spine.
   Adding a module must never require editing core code. If it seems to,
   stop and say so, because that means the spine is wrong.
5. **One database, one set of conventions.** All modules share the same
   database and follow the same naming, timestamp, and ID conventions so
   the brain can query across them later.
6. **Ask before adding dependencies.** Name the package, say why, and
   wait. Fewer dependencies is better.

## Security

Handled per module as it is built, never deferred to later.

- Every backend endpoint requires authentication. No exceptions, no
  temporary open endpoints for testing.
- Secrets and API keys live in `.env` only. Never hardcode them, never
  log them, never commit them. Keep `.env.example` updated with key names
  and no values.
- The database is never directly reachable from outside the backend.
- Validate all input at the edge. Treat anything coming through capture,
  including text the brain parses, as untrusted.
- When finishing a module, list its security considerations and confirm
  each one is handled.

## How to work

- Plan before code for anything beyond a small change. Propose the
  approach, wait for approval, then build.
- Work in small, reviewable chunks. When a chunk works and is tested, say
  so plainly so it can be committed.
- Stay inside the task. Do not refactor, rename, or "improve" code outside
  the current scope. Suggest it instead. Don't lose capability to ultrathink code.
- Test endpoints as they are built and show the curl command used.
- When unsure about intent, ask. Do not guess on anything that touches
  the data model, auth, or the spine.
- Be honest about problems, limitations, and shortcuts taken. Never hide
  a hack.

## Communication

Short and direct. Honest, including about technical limitations. In prose,
avoid hyphens.

## Current phase: headless MVP

Build order:

1. Spine, data layer, auth
2. Tasks module
3. Terminal readout script (plain text dashboard, the fake UI)
4. Action button capture endpoint
5. The brain, wired into tasks
6. Daily use for two weeks
7. Module two
8. Dashboard and UI

Only work on the current step unless told otherwise.

## Stack

Approved 2026-10-11.

- **Language:** Python 3.13.
- **Framework:** FastAPI, run by uvicorn (plain install, no `[standard]`
  extras). FastAPI's built in OpenTelemetry support is hard disabled.
- **Database:** SQLite through stdlib `sqlite3`. No ORM. STRICT tables.
  Migrations are plain SQL files per module, run by our own runner.
- **Packages:** stdlib `venv` in `.venv/`. Every package pinned exactly in
  `requirements.txt` (runtime) and `requirements-dev.txt` (pytest, httpx).
- **Auth:** static bearer tokens, one per device or channel, stored in
  `.env` as SHA256 hashes only.
- **Deployment:** home Mac mini as a `launchd` service. How it is reached
  from outside is decided in step 4 (Tailscale recommended).

## Later: UI components

When the UI phase starts, components will live in a structured file
hierarchy, filed by keyword. When a component is mentioned by name, find
it in that hierarchy and use it rather than writing a new one. All
components get normalized to shared design tokens (color, type, spacing,
radius, shadow). Ignore this section until the UI phase.