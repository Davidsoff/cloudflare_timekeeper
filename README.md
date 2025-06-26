# Cloudflare Timekeeper Monorepo

## Structure

- `timekeeper-frontend/` — SvelteKit frontend (public and admin UI)
- `timekeeper-backend/`
  - `workers/`
    - `core/` — Core worker (example, can be split further)
    - `auth/` — Authentication and user management
    - `event/` — Event and location management
    - `crew/` — Crew and crew member management
    - `distance/` — Distances and splits per event
    - `timing/` — Start, finish, and split time registration
    - `penalty/` — Penalties (time, DQ, warnings, custom)
    - `finance/` — Payments, invoices, finance officer actions
    - `results/` — Results calculation and display
    - `static-renderer/` — Static page/API generation for R2
  - `shared/` — Common types and utilities for all workers

## How to Use

- Each worker is a separate Cloudflare Worker with its own `wrangler.toml`.
- Shared types/utilities are in `timekeeper-backend/shared` and can be imported as `@timekeeper/shared`.
- Use `pnpm` for workspace management and dependency installation.

## Next Steps
- Implement database migrations and API endpoints per worker.
- Add tests for each worker and shared logic.
- See `docs/` for architecture diagrams and requirements. 