# 006 — Implement email, analytics, and verification harness

## Goal
Complete operational foundation for local agents and production deployment.

## Inputs
- `docs/specs/EMAIL_SPEC.md`
- `docs/specs/ANALYTICS_SPEC.md`
- `docs/harness/HARNESS_SPEC.md`

## Scope
- Console email provider.
- Mailgun provider.
- Transactional email templates.
- Analytics tracker and admin dashboard.
- Non-interactive local setup script.
- Expanded smoke tests.

## Definition of done
- `pnpm setup:local` creates a runnable local app.
- `pnpm verify` passes without Stripe/Mailgun credentials.
- Admin analytics page displays session/page-view data.
