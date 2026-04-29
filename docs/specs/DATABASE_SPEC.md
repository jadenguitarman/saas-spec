# DATABASE_SPEC.md

## Goal
Define the reusable database foundation for a SaaS starter using Drizzle ORM and Postgres.

## Core tables

### Identity and teams
- `users`
- `teams`
- `team_members`
- `invitations`
- `activity_logs`
- `password_reset_tokens`

### Billing
Billing fields live on `teams`:
- `stripeCustomerId`
- `stripeSubscriptionId`
- `stripeProductId`
- `planName`
- `subscriptionStatus`
- `trialEnd`
- `eligibleForTrial`

### Analytics
- `sessions`
- `page_views`
- optional `events`

### Product data placeholder
The starter should include one tiny demo table only if useful for smoke testing, for example:
- `demo_items`

Do not include Load Calc Guru tables for projects, rooms, ducts, windows, doors, equipment, permits, leads, or thermal inference.

## Drizzle module layout

```text
lib/db/
  drizzle.ts
  schema.ts
  migrations/
  setup.ts
  seed.ts
lib/queries/
  user.ts
  team.ts
  billing.ts
  analytics.ts
```

## Connection behavior
- Read `POSTGRES_URL` from environment.
- Reuse global Postgres client in development to avoid connection churn.
- Keep production pool conservative.
- Migrations are generated with `drizzle-kit generate` and applied with `drizzle-kit migrate`.

## Naming rules
- Use lowercase snake_case database column names unless preserving existing public API compatibility.
- Use TypeScript camelCase fields in Drizzle definitions where appropriate.
- Prefer text UUID-style IDs generated in application code.
- All user-owned records must include owner/team references.

## Query/service rules
- UI components must not contain complex database logic.
- API routes should call query/service modules.
- Multi-table signup and billing updates must use transactions where consistency matters.
- Subscription webhook handlers must be idempotent.

## Local harness
Required scripts:

```json
{
  "db:setup": "node --import tsx lib/db/setup.ts",
  "db:generate": "drizzle-kit generate",
  "db:migrate": "drizzle-kit migrate",
  "db:studio": "drizzle-kit studio",
  "db:seed": "node --import tsx lib/db/seed.ts"
}
```

Non-interactive local setup must:
- Create or reuse a Docker Postgres container.
- Write `.env` from `.env.example` defaults when safe.
- Run migrations.
- Seed a dev user.

## Acceptance criteria
- `pnpm db:setup && pnpm db:migrate && pnpm db:seed` creates a usable local app.
- `pnpm verify` can run in a clean agent workspace with local Postgres.
- Schema contains only reusable SaaS core tables plus minimal demo tables.
