# 003 — Implement teams and user management

## Goal
Add team ownership, membership, invitations, settings, and admin user screens.

## Inputs
- `docs/specs/USER_MANAGEMENT_SPEC.md`

## Scope
- Teams and team_members tables.
- Invitation table and flow.
- User/team context provider.
- User settings page.
- Team settings page.
- Admin users page.

## Definition of done
- Signup creates owner team.
- Owner can invite/remove members.
- Admin users page is protected.
- Client UI uses one shared logged-in user context.
