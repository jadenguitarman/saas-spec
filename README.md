# SaaS Starter Spec Pack

Generated from the provided Load Calc Guru repo snapshot.

This pack is meant to be copied into a new SaaS starter repo before implementation. It separates reusable SaaS infrastructure from Load Calc Guru-specific domain logic and emphasizes agent-friendly harness design.

## Files

```text
START.md
RULES.md
AGENTS.md
docs/specs/STARTER_ARCHITECTURE.md
docs/specs/AUTH_SPEC.md
docs/specs/DATABASE_SPEC.md
docs/specs/BILLING_SPEC.md
docs/specs/USER_MANAGEMENT_SPEC.md
docs/specs/PAGE_STRUCTURE_SPEC.md
docs/specs/BLOG_SPEC.md
docs/specs/EMAIL_SPEC.md
docs/specs/ANALYTICS_SPEC.md
docs/harness/HARNESS_SPEC.md
tasks/001-bootstrap-repo.md
tasks/002-auth.md
tasks/003-teams-users.md
tasks/004-billing.md
tasks/005-blog-marketing.md
tasks/006-email-analytics-harness.md
```

## Recommended use
1. Put these files into the root of a fresh SaaS starter repo.
2. Start with `tasks/001-bootstrap-repo.md`.
3. Implement each task in order.
4. Keep domain app code out of the SaaS core.
