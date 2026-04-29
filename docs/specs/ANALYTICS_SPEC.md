# ANALYTICS_SPEC.md

## Goal
Provide lightweight first-party analytics based on Load Calc Guru's `sessions` and `page_views` pattern.

## Required features
- Anonymous session tracking.
- Optional user association when logged in.
- Page view capture.
- Admin dashboard summary.
- Basic top pages and active sessions metrics.

## Data model

### `sessions`
- `id text primary key`
- `userId text nullable references users(id)`
- `createdAt timestamp default now not null`
- `updatedAt timestamp default now not null`
- `userAgent text nullable`
- `ipAddress varchar(45) nullable`

### `page_views`
- `id text primary key`
- `sessionId text not null references sessions(id) on delete cascade`
- `path text not null`
- `referrer text nullable`
- `timestamp timestamp default now not null`

Optional later:
- `events` table with `name`, `properties`, `timestamp`.

## Required modules

```text
components/core/analytics/AnalyticsTracker.tsx
lib/queries/analytics.ts
app/api/analytics/track/route.ts
app/admin/analytics/page.tsx
```

## Privacy rules
- Keep data minimal.
- Avoid storing raw bodies or sensitive form data.
- Do not use analytics as an audit log. Use `activity_logs` for account actions.
- Allow analytics to be disabled with `ENABLE_ANALYTICS=false`.

## Admin dashboard metrics
- total sessions in selected time range
- total page views
- percent logged-in sessions
- top pages
- recent sessions with user email when available

## Acceptance criteria
- Page views are recorded in local dev when enabled.
- Logged-in user sessions are associated with `userId`.
- Admin analytics page is protected.
- Analytics route tolerates missing/blocked cookies without breaking navigation.
