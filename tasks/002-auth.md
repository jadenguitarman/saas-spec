# 002 — Implement auth core

## Goal
Implement reusable email/password auth.

## Inputs
- `docs/specs/AUTH_SPEC.md`
- `docs/specs/DATABASE_SPEC.md`

## Scope
- Users table.
- Password reset tokens table.
- Session JWT helpers.
- Signup/sign-in/reset pages.
- Authenticated guard helpers.
- Seeded dev user.

## Definition of done
- Dev user can sign in.
- New user can sign up.
- Dashboard requires auth.
- Password reset works with console email.
- Smoke tests cover password/session utilities.
