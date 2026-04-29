# START.md — SaaS Starter Agent Entry Point

## Purpose
Build a reusable Next.js SaaS starter derived from Load Calc Guru's proven scaffolding, without importing HVAC/manual-J/permit-specific product logic.

This repo should let an agent or developer spin up a working SaaS instance quickly with auth, team billing, user management, blog/marketing pages, email, analytics, and a clean feature module boundary.

## Reference implementation
Use Load Calc Guru as a reference for patterns, not as a codebase to clone wholesale.

Reusable patterns to extract:
- Next.js App Router structure with route groups.
- Custom JWT session stored in an HTTP-only cookie.
- Drizzle ORM with Postgres.
- Team-based Stripe subscription model.
- Logged-in user/team context provider.
- Paywall and upgrade components.
- React Email + Mailgun sending layer.
- Markdown blog pipeline.
- Admin/user management screens.
- Workspace verification and smoke-test harness.

Product-specific logic to exclude:
- HVAC/manual-J/manual-S/manual-D calculations.
- Project rooms, ducts, fixtures, weather stations, building materials, equipment catalogs.
- Permit board scraping and lead-generation workflows unless building a generic CRM/marketing module later.
- Load Calc Guru copy, branding, blog content, reports, PDFs, and sample assets.

## First run target
A fresh checkout must support:

```bash
pnpm install
cp .env.example .env
pnpm db:setup
pnpm db:migrate
pnpm dev
pnpm verify
```

For CI and agent environments, there must also be a non-interactive path:

```bash
pnpm setup:local
pnpm verify
```

## Required starter capabilities
1. Public marketing site.
2. Sign up, sign in, sign out, password reset.
3. Dashboard behind auth.
4. Team and user model.
5. Stripe subscription billing and customer portal.
6. Free/trial/active/expired subscription states.
7. Paywall gate for feature-level gating.
8. User settings and team member management.
9. Blog using Markdown files.
10. Transactional email layer.
11. Basic first-party analytics.
12. Admin panel for users and analytics.
13. Harness scripts for local database, seeded test user, and smoke tests.

## Agent operating rules
- Preserve a strict SaaS core versus domain module split.
- Do not add product-specific code to `lib/core`, `components/core`, or shared layouts.
- Do not introduce new vendors if an existing abstraction is enough.
- All DB access goes through Drizzle and query/service modules.
- All app behavior that requires secrets must have a local fake/test mode.
- Every feature must have a local verification path that does not require production Stripe, Mailgun, or Vercel.

## Definition of done
The starter is done when an agent can create a new SaaS app by adding a domain module and changing branding/config, without touching auth, billing, blog, user management, or harness internals.
