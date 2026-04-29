# RULES.md — SaaS Starter Development Rules

## Architecture rules
- Keep reusable SaaS infrastructure separate from application domain logic.
- Preferred structure:
  - `app/` for Next.js routes and route handlers.
  - `components/ui/` for shadcn/Radix primitives.
  - `components/core/` for reusable SaaS components.
  - `components/<feature>/` for domain-specific UI.
  - `lib/core/` for reusable auth, billing, email, analytics, and bootstrap logic.
  - `lib/<feature>/` for domain logic.
  - `lib/db/schema.ts` for Drizzle schema definitions.
  - `lib/queries/` or `lib/services/` for database/business operations.
  - `types/` for shared TypeScript types.
- Route handlers should be thin: validate input, call service/query layer, return response.
- Server Components may call query modules directly. Client Components must call route handlers or server actions.

## Database rules
- Use Drizzle ORM for schema, queries, migrations, and typed models.
- Never edit generated migrations manually unless explicitly repairing a migration.
- Use soft delete for user-owned business records where recovery matters.
- Use `createdAt`, `updatedAt`, and where relevant `deletedAt` on persistent models.
- Store Stripe state on the team/account billing entity, not on individual feature records.

## Authentication rules
- The starter uses custom JWT sessions in HTTP-only cookies unless replaced intentionally across the whole repo.
- Session lookup must be centralized.
- Authenticated actions must use shared helpers rather than reimplementing cookie checks.
- Password hashes must use bcrypt or a stronger password hashing strategy.
- Password reset tokens must be stored hashed, expire, and be single-use.

## Billing rules
- Billing is team/account-scoped.
- The database mirrors Stripe subscription state; Stripe remains the payment source of truth.
- Webhooks must verify signatures before processing.
- Feature gating reads normalized subscription state, not raw Stripe objects.
- Customer portal is preferred for subscription/payment method management after signup.

## UI rules
- Use shadcn/Radix primitives, Tailwind, and lucide-react.
- Keep layout primitives reusable and app-specific pages thin.
- Handle loading, empty, error, and unauthorized states explicitly.
- Avoid one-off styling systems.

## Email rules
- Email templates live in React components.
- Email senders call a central email service.
- Local development must support logging emails instead of sending them.
- Never send outbound email in local/test unless the recipient is explicitly allowlisted.

## Harness rules
- Every repo must include `.env.example`, `docker-compose.yml`, `scripts/check-workspace.ts`, `scripts/smoke.ts`, and package scripts for setup and verification.
- Agents must run `pnpm check:workspace` before broad edits.
- Agents must run `pnpm verify` before handoff.
- Infrastructure-touching changes require `pnpm verify:full`.
- The repo must support non-interactive setup for agents/CI.

## Anti-patterns
- Do not copy Load Calc Guru domain tables into the starter.
- Do not bury generic auth/billing logic in feature modules.
- Do not make local development depend on live Stripe webhooks or live Mailgun.
- Do not let agents add libraries to solve problems already covered by the stack.
- Do not use a monolithic `SPEC.md` as the only source of implementation truth.
