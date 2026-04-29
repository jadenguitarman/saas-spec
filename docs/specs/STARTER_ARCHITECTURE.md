# STARTER_ARCHITECTURE.md

## Goal
Create a reusable SaaS starter that captures Load Calc Guru's SaaS infrastructure patterns while removing domain-specific HVAC logic.

## Stack
- Next.js App Router.
- React 19-compatible components.
- TypeScript strict mode.
- Tailwind CSS.
- shadcn/Radix UI primitives.
- Drizzle ORM.
- Postgres.
- Stripe for billing.
- React Email for templates.
- Mailgun or pluggable email provider.
- SWR for client-side user/team hydration where useful.
- Vercel-friendly deployment, but not Vercel-locked.

## Recommended folder structure

```text
app/
  (with-nav)/
    layout.tsx
    page.tsx
    dashboard/
  (without-nav)/
    sign-in/
    sign-up/
    reset-password/
  (marketing)/ or (public)/
    pricing/
    blog/
    privacy/
    terms/
  admin/
  api/
    auth/
    billing/
    user/
    team/
    analytics/
    blog-og/
components/
  core/
    auth/
    billing/
    layout/
    marketing/
    user/
  emails/
  ui/
lib/
  core/
    auth/
    billing/
    email/
    analytics/
    config/
  db/
  queries/
  services/
  blog.ts
  utils.ts
types/
scripts/
  check-workspace.ts
  smoke.ts
  setup-local.ts
  seed-dev.ts
blog/
docs/
  specs/
  harness/
```

## Core modules

### Auth core
Responsibilities:
- User creation.
- Password hashing.
- Login credential validation.
- JWT cookie creation and verification.
- Session lookup.
- Password reset token lifecycle.
- Authenticated action wrappers.

### Account/team core
Responsibilities:
- Team creation at signup.
- Owner/member relationships.
- Invitations.
- Role checks.
- Team settings.
- Activity logs.

### Billing core
Responsibilities:
- Stripe customer creation.
- Checkout or payment method capture.
- Subscription creation.
- Customer portal session creation.
- Stripe webhook processing.
- Subscription state normalization.
- Paywall decisions.

### Email core
Responsibilities:
- Provider abstraction.
- React Email rendering.
- Local dev logging transport.
- Allowlist safeguards.
- Transactional templates for welcome, reset password, invitation, trial ending, payment failed.

### Blog/marketing core
Responsibilities:
- Markdown parsing.
- Slug routing.
- Metadata generation.
- OG image route.
- Sitemap support.

### Analytics core
Responsibilities:
- Session tracking.
- Page view tracking.
- Admin analytics dashboard.
- Privacy-conscious first-party event capture.

## Routing pattern
Use route groups to separate concerns:

- `(with-nav)`: routes that use the application shell/navbar.
- `(without-nav)`: auth and focused conversion flows.
- `(marketing)` or `(public)`: public marketing pages.
- `admin`: admin-only pages.
- `api`: route handlers.

## Data flow
- Server Components call query/service modules directly.
- Client Components receive initial props or call API route handlers.
- Mutations validate input with Zod, then call service modules.
- Subscription gating is done with normalized state exposed by a logged-in user provider.

## Product module contract
A new SaaS app should add domain-specific code under:

```text
components/<domain>/
lib/<domain>/
types/<domain>.ts
app/api/<domain>/
app/(with-nav)/dashboard/<domain>/
```

The domain module may depend on core modules. Core modules must not depend on the domain module.

## Non-goals
- Do not include Load Calc Guru HVAC logic.
- Do not include lead scraping or permit automation in the baseline starter.
- Do not create a complex plugin system before one is needed.
- Do not support multiple databases in v1.
