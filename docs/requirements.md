# Requirements

## User Management
- Invitation-based registration: Super admins invite users; users can reset passwords.
- Users can have only one role per event, but can be linked to multiple events.
- Roles: super admin, event administrator, judge, timekeeper, finance officer.
- Password reset/forgot password flows required.
- Configurable email service for invitations and password resets.

## Event & Location Structure
- Locations represent physical places (e.g., Bodbaan).
- Events are linked to locations; multiple events can be co-located and overlap in time.

## Timing & Results
- Arbitrary split times per event (can be none).
- Penalties: time-based, disqualification, exclusion, warnings, and custom per event (event admins can define custom penalty types via UI).
- Results page must be dynamically updated when new results come in.

## Finance
- Finance officer can mark payments (per crew or club).
- Finance officer can view/download invoices (per crew or club) in PDF format.
- (Future) Generate payment link and QR code (not required for MVP).

## Public API & Rate Limiting
- Public API for entries, draw, results.
- Open but rate-limited (10 rps per user).
- Cloudflare bot/scraping protection.

## Tech Stack
- Backend: Cloudflare Workers (one per responsibility), D1 for relational data, R2 for static assets/pages.
- Frontend: Svelte, mobile-friendly from the start.
- i18n: English and Dutch, easily extensible.
- All code (backend and frontend) must have relevant tests.

## Static Rendering
- Public pages and API responses are statically rendered and served from R2.
- Static pages are regenerated and uploaded to R2 after relevant data changes.

## Branding & UI
- Color palette: orange and white, with dark mode and color-blind friendly design.
- Use a neutral palette for other elements as needed.
- Svelte frontend, mobile-friendly.

## Miscellaneous
- Results page must be dynamically updated (polling or WebSockets).
- All requirements tracked in this file. 