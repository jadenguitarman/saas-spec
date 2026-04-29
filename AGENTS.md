# AGENTS.md — SaaS Starter Blueprint

## What this repo is
A reusable SaaS starter extracted from Load Calc Guru's SaaS infrastructure patterns.

The starter should provide a complete app shell: auth, teams, billing, user management, blog, email, analytics, admin screens, and local/CI harness.

## What this repo is not
It is not Load Calc Guru. Do not copy HVAC calculation logic, permit workflows, equipment catalogs, reports, or product-specific content into the starter.

## Where to start
1. Read `START.md`.
2. Read `RULES.md`.
3. Read the relevant spec in `docs/specs/`.
4. Run `pnpm check:workspace` before broad edits.
5. Keep the change scoped.
6. Run `pnpm verify` before handoff.

## Core specs
- `docs/specs/STARTER_ARCHITECTURE.md`
- `docs/specs/AUTH_SPEC.md`
- `docs/specs/DATABASE_SPEC.md`
- `docs/specs/BILLING_SPEC.md`
- `docs/specs/USER_MANAGEMENT_SPEC.md`
- `docs/specs/PAGE_STRUCTURE_SPEC.md`
- `docs/specs/BLOG_SPEC.md`
- `docs/specs/EMAIL_SPEC.md`
- `docs/specs/ANALYTICS_SPEC.md`
- `docs/harness/HARNESS_SPEC.md`

## Implementation principles
- Core SaaS infrastructure must remain domain-agnostic.
- Domain apps should add feature modules rather than modifying core modules.
- Prefer boring, explicit code over clever abstractions.
- Local development must work without production credentials.
- External services must have fake or console modes where practical.
