# 004 — Implement billing core

## Goal
Add Stripe-ready team billing with local fake mode.

## Inputs
- `docs/specs/BILLING_SPEC.md`

## Scope
- Billing fields on teams.
- Stripe service module.
- Fake billing provider.
- Pricing route.
- Webhook route.
- Billing settings.
- Upgrade button.
- Paywall gate.

## Definition of done
- Fake billing lets local dev switch free/trialing/active states.
- Real Stripe mode verifies webhooks.
- Paywall gate responds to normalized subscription state.
- Customer portal session works when Stripe env is configured.
