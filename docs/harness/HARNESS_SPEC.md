# HARNESS_SPEC.md — Agent-Friendly Runtime Harness

## Goal
Make the SaaS starter easy for agents and developers to run, verify, and modify without manual environment archaeology.

The repo should be optimized for fast spin-up, deterministic local state, clear failure messages, and service fakes where production credentials are unavailable.

## Required commands

```json
{
  "dev": "next dev --turbopack",
  "build": "next build",
  "start": "next start",
  "lint": "eslint . --ext .js,.jsx,.ts,.tsx --cache",
  "db:setup": "node --import tsx lib/db/setup.ts",
  "db:generate": "drizzle-kit generate",
  "db:migrate": "drizzle-kit migrate",
  "db:studio": "drizzle-kit studio",
  "db:seed": "node --import tsx lib/db/seed.ts",
  "setup:local": "node --import tsx scripts/setup-local.ts",
  "check:workspace": "node --import tsx scripts/check-workspace.ts",
  "test:smoke": "node --import tsx scripts/smoke.ts",
  "verify": "pnpm check:workspace && pnpm lint && pnpm test:smoke",
  "verify:full": "pnpm verify && pnpm build"
}
```

## Required files

```text
.env.example
docker-compose.yml
AGENTS.md
WORKFLOW.md
scripts/check-workspace.ts
scripts/smoke.ts
scripts/setup-local.ts
lib/db/setup.ts
lib/db/seed.ts
```

## Local setup modes

### Interactive mode
`pnpm db:setup` may prompt for app name, domain, Postgres URL, Stripe keys, and email provider.

### Non-interactive agent mode
`pnpm setup:local` must:
- Create `.env` if missing.
- Fill safe local defaults.
- Start Docker Postgres.
- Run migrations.
- Seed dev user/team.
- Configure fake billing and console email by default.

Default local env:

```env
APP_NAME=SaaS Starter
APP_DOMAIN=http://localhost:3000
POSTGRES_URL=postgres://postgres:postgres@localhost:54322/postgres
AUTH_SECRET=local-dev-auth-secret-change-me
BILLING_PROVIDER=fake
EMAIL_PROVIDER=console
ENABLE_ANALYTICS=true
TRIAL_PERIOD_DAYS=7
```

## Docker
Use Postgres only by default:

```yaml
services:
  postgres:
    image: postgres:16.4-alpine
    ports:
      - "54322:5432"
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
```

## Smoke tests
Smoke tests should cover reusable starter behavior, not product domain calculations.

Minimum checks:
- Env parsing.
- Password hashing and verification.
- JWT session sign/verify.
- Subscription state normalization.
- Email provider console mode.
- Blog Markdown parsing for a fixture post.
- Database connection if `POSTGRES_URL` is set.

## Workspace check
`check-workspace.ts` should report:
- Required files present.
- Node available.
- pnpm available.
- package scripts present.
- `.env.example` has required keys.
- `WORKFLOW.md` front matter is valid if using a workflow harness.
- Optional secrets status grouped by category.

It should fail only on missing essentials. Missing external API keys should be warnings unless a task requires them.

## Service fakes
Required fakes:
- Billing fake: set team subscription status locally.
- Email fake: log rendered emails to console or `.local/emails`.
- Analytics can run locally or be disabled.

Optional fakes:
- Webhook fixture runner.
- Test payment method simulation.

## Agent handoff expectations
Every agent final summary should include:
- Files changed.
- Verification commands run.
- Results.
- Any missing secrets or external dependencies.
- Residual risk.

## Acceptance criteria
- A fresh machine with Node, pnpm, and Docker can run the app locally in under one command after install.
- An agent can run `pnpm verify` without real Stripe or Mailgun credentials.
- Failures explain exactly what is missing and how to fix it.
