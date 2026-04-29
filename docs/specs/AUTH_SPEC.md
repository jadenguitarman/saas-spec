# AUTH_SPEC.md

## Goal
Implement a simple, durable authentication layer based on Load Calc Guru's custom JWT-cookie pattern.

## Required features
- Email/password signup.
- Email/password sign-in.
- HTTP-only session cookie.
- Sign-out.
- Password reset request.
- Password reset confirmation.
- Authenticated user lookup.
- Authenticated action wrapper.
- Optional admin flag.

## Data model

### `users`
Required fields:
- `id text primary key`
- `name varchar(100)` nullable
- `email varchar(255) not null unique`
- `passwordHash text not null`
- `createdAt timestamp default now not null`
- `updatedAt timestamp default now not null`
- `deletedAt timestamp nullable`
- `hasCompletedOnboarding boolean default false not null`
- `isAdmin boolean default false not null`

### `password_reset_tokens`
Required fields:
- `id text primary key`
- `userId text not null references users(id) on delete cascade`
- `tokenHash text not null unique`
- `expiresAt timestamp not null`
- `createdAt timestamp default now not null`
- `usedAt timestamp nullable`

## Session model
- JWT signed with `AUTH_SECRET`.
- Stored in cookie named `session`.
- Cookie options:
  - `httpOnly: true`
  - `secure: true` in production
  - `sameSite: lax`
  - expiry defaults to 1 day unless product decides otherwise
- Payload contains:

```ts
{
  user: { id: string },
  expires: string
}
```

## Required modules

```text
lib/core/auth/session.ts
lib/core/auth/password.ts
lib/core/auth/actions.ts
lib/core/auth/guards.ts
lib/queries/user.ts
app/(without-nav)/sign-in/page.tsx
app/(without-nav)/sign-up/page.tsx
app/(without-nav)/reset-password/page.tsx
app/api/auth/sign-out/route.ts
```

## Required behaviors
- Signup creates `users`, `teams`, and owner `team_members` row in a transaction.
- Login rejects deleted users.
- Password reset never reveals whether an email exists.
- Reset tokens are random, hashed before storage, single-use, and expire.
- Authenticated server actions use a shared wrapper that loads the user and rejects unauthenticated access.
- Dashboard layout redirects unauthenticated users to `/sign-in`.

## Local dev harness
- `pnpm seed:dev` creates a deterministic test user:
  - email: `dev@example.com`
  - password: `password1234`
- Smoke test verifies:
  - password hashing/checking
  - session signing/verification
  - expired reset token rejection

## Acceptance criteria
- A user can sign up, sign in, access `/dashboard`, sign out, and be denied dashboard access afterward.
- Password reset flow works using local email logging.
- Auth utilities are centralized; no route duplicates JWT verification logic.
