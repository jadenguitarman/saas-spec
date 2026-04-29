# EMAIL_SPEC.md

## Goal
Implement a reusable transactional email system based on Load Calc Guru's React Email component pattern and central email sender.

## Provider model
The starter should support:
- `EMAIL_PROVIDER=console` for local/dev.
- `EMAIL_PROVIDER=mailgun` for production.

The code should be structured so another provider can be added later without rewriting templates.

## Required modules

```text
lib/core/email/send.ts
lib/core/email/provider.ts
lib/core/email/safeguards.ts
components/emails/EmailLayout.tsx
components/emails/WelcomeEmail.tsx
components/emails/PasswordResetEmail.tsx
components/emails/TeamInvitationEmail.tsx
components/emails/TrialEndingEmail.tsx
components/emails/SubscriptionFailedEmail.tsx
```

## Send API
Central sender shape:

```ts
sendEmail({
  to: string,
  subject: string,
  react?: ReactElement,
  text?: string,
  html?: string,
  tags?: string[],
})
```

Rules:
- Prefer React templates for transactional emails.
- Generate text fallback where practical.
- All outbound sends go through this service.
- No route/component should call Mailgun directly.

## Local safeguards
- In development, default to console/log transport.
- If real provider is enabled locally, only send to addresses or domains listed in allowlist env vars.
- Never send marketing or transactional email to arbitrary recipients from local agent environments.

Env vars:
- `EMAIL_PROVIDER=console|mailgun`
- `MAILGUN_API_KEY`
- `MAILGUN_DOMAIN`
- `MAILGUN_WEBHOOK_SIGNING_KEY`
- `EMAIL_FROM`
- `LOCAL_OUTBOUND_ALLOWED_EMAILS`
- `LOCAL_OUTBOUND_ALLOWED_DOMAINS`

## Required transactional emails
- Welcome after signup.
- Password reset.
- Team invitation.
- Trial ending.
- Subscription/payment failed.

## Acceptance criteria
- Local password reset logs a reset link instead of sending.
- Production mode sends through Mailgun.
- All sends are centralized.
- Templates share a consistent layout.
