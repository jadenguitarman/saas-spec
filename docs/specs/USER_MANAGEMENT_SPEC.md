# USER_MANAGEMENT_SPEC.md

## Goal
Provide reusable account, team, invitation, role, settings, and admin-user management patterns.

## Core concepts
- `user`: an individual identity.
- `team`: the billing/account workspace.
- `team_member`: relationship between user and team.
- `role`: user's permission level within a team.
- `activity_log`: audit trail for important user/team actions.

## Required roles
Start simple:
- `owner`: manages billing, team members, and account settings.
- `member`: uses the product.
- `admin`: platform-level support/admin role stored on `users.isAdmin`.

Avoid adding complex RBAC until a real product needs it.

## Required features
- Team is created at signup.
- Signup user becomes team owner.
- User can update name/profile.
- Owner can invite team members.
- Owner can remove team members.
- Owner can open billing settings/customer portal.
- Admin can view users and basic user metrics.
- Admin pages are protected by `isAdmin`.

## Data model

### `teams`
- `id`
- `name`
- `createdAt`
- `updatedAt`
- billing fields from `BILLING_SPEC.md`

### `team_members`
- `id`
- `userId`
- `teamId`
- `role`
- `joinedAt`

### `invitations`
- `id`
- `teamId`
- `email`
- `role`
- `invitedBy`
- `invitedAt`
- `status`
- optional `tokenHash`, `expiresAt`, `acceptedAt`

### `activity_logs`
- `id`
- `teamId`
- `userId`
- `action`
- `timestamp`
- `ipAddress`

## Required modules

```text
lib/queries/user.ts
lib/queries/team.ts
lib/services/invitations.ts
lib/services/activity-log.ts
components/core/user/UserSettings.tsx
components/core/team/TeamSettings.tsx
components/core/team/InviteMemberDialog.tsx
app/api/user/route.ts
app/api/team/route.ts
app/admin/users/page.tsx
```

## User context
Expose a logged-in user context to client UI with:
- `user`
- `team`
- `teamMembers`
- `subscriptionState`
- `isOnPaidPlan`
- `trialEndsAt`
- `refreshUser`

This mirrors Load Calc Guru's `LoggedInUserContext` pattern and prevents subscription checks from being duplicated across random components.

## Admin user metrics
Starter admin panel should show:
- user email/name
- signup date
- team name
- subscription status
- Stripe customer id
- basic product usage counter if a product module provides one

Do not hardcode Load Calc Guru metrics like Manual S usage.

## Acceptance criteria
- New signup has exactly one team and owner membership.
- Owner can invite/remove member in local dev.
- Member cannot manage billing or remove owner.
- Admin route rejects normal users.
- Client UI gets user/team/billing state from one context provider.
