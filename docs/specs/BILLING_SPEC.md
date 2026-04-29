# BILLING_SPEC.md

## Goal
Implement team-based Stripe billing modeled after Load Calc Guru's subscription architecture.

## Billing model
- A team/account owns the subscription.
- A user belongs to a team through `team_members`.
- Feature access is derived from the team's normalized subscription state.
- Stripe is the payment source of truth; the database stores the local mirror needed for fast access checks.

## Data fields on `teams`
- `stripeCustomerId text unique nullable`
- `stripeSubscriptionId text unique nullable`
- `stripeProductId text nullable`
- `planName varchar(50) nullable`
- `subscriptionStatus varchar(20) default 'free' not null`
- `trialEnd timestamp nullable`
- `eligibleForTrial boolean default true not null`

## Normalized subscription states
Expose these values to UI and feature gates:
- `free`
- `trialing`
- `active`
- `past_due`
- `canceled`
- `expired`
- `unpaid`

Computed helpers:
- `isOnPaidPlan`
- `isTrialing`
- `isTrialExpired`
- `canStartTrial`
- `trialEndsAt`

## Required modules

```text
lib/core/billing/stripe.ts
lib/core/billing/state.ts
lib/core/billing/actions.ts
components/core/billing/BillingSettings.tsx
components/core/billing/UpgradeButton.tsx
components/core/billing/PaywallGate.tsx
app/api/billing/pricing/route.ts
app/api/stripe/webhook/route.ts
```

## Stripe integration
Required operations:
- Create customer.
- Attach payment method or redirect to checkout.
- Create subscription.
- Create customer portal session.
- Fetch pricing for active products/prices.
- Process subscription webhooks.

Required webhook events:
- `customer.subscription.updated`
- `customer.subscription.deleted`
- optionally `customer.subscription.created`
- optionally `invoice.payment_failed`

## Pricing source
Preferred pattern:
- Filter Stripe products by `APP_NAME` or product metadata.
- Read bullets, limits, CTA labels, and slugs from product metadata.
- Cache pricing response briefly.

## Paywall pattern
`PaywallGate` wraps specific feature entry points, not the entire dashboard.

Behavior:
- Render children when subscription is `active` or `trialing`.
- Show start-trial CTA if free and eligible.
- Show upgrade/manage-billing CTA if expired, canceled, unpaid, or past_due.

## Customer portal
Use Stripe Billing Portal for subscription changes, cancellations, and payment method updates after initial signup.

## Local/test harness
- Billing must run in fake mode when `BILLING_PROVIDER=fake`.
- Fake mode should support setting a test team to `free`, `trialing`, `active`, or `past_due` without contacting Stripe.
- Stripe webhook verification must be covered by an integration-style test or smoke fixture.

## Acceptance criteria
- A dev user can start in `free`, enter `trialing`, and be set to `active` locally.
- UI gates respond to normalized billing state.
- Webhook route verifies signatures in real mode.
- No feature module calls Stripe directly.
