# PAGE_STRUCTURE_SPEC.md

## Goal
Define a reusable page and route structure for the SaaS starter using the Load Calc Guru App Router layout pattern.

## Route groups

```text
app/
  (with-nav)/
    layout.tsx
    page.tsx
    dashboard/
      page.tsx
  (without-nav)/
    layout.tsx
    sign-in/
      page.tsx
    sign-up/
      page.tsx
    reset-password/
      page.tsx
  (marketing)/
    page.tsx
    pricing/
      page.tsx
    blog/
      page.tsx
      [slug]/page.tsx
    privacy/page.tsx
    terms/page.tsx
    contact/page.tsx
  admin/
    layout.tsx
    users/page.tsx
    analytics/page.tsx
  api/
```

## Layout responsibilities

### Root layout
- Global CSS.
- Metadata defaults.
- Theme provider.
- Toast provider.
- Analytics tracker.

### `(with-nav)` layout
- Main navigation.
- Logged-in user provider.
- Trial/subscription status display.
- Shared app shell.

### `(without-nav)` layout
- Auth pages and focused payment/conversion pages.
- No main nav.
- Minimal footer or none.

### `(marketing)` layout
- Public navbar.
- Marketing footer.
- SEO-friendly pages.

### `admin` layout
- Admin navbar/sidebar.
- Server-side admin guard.

## Core pages
- `/`: marketing home.
- `/pricing`: pricing table from billing API.
- `/blog`: blog index.
- `/blog/[slug]`: blog detail.
- `/sign-in`: login.
- `/sign-up`: account creation.
- `/reset-password`: reset request and confirmation.
- `/dashboard`: authenticated app home.
- `/settings`: user/team settings.
- `/admin/users`: admin user management.
- `/admin/analytics`: basic analytics dashboard.

## Navigation components
Recommended reusable components:
- `components/core/layout/AppNavbar.tsx`
- `components/core/layout/PublicNavbar.tsx`
- `components/core/layout/AppFooter.tsx`
- `components/core/layout/TrialCountdown.tsx`
- `components/core/layout/UserMenu.tsx`

## Page rules
- Page components should be thin.
- Complex forms live in feature/core components.
- Server-side data loading belongs in page/layout server components or query modules.
- Client components should receive compact props or fetch from route handlers.

## Marketing page rules
- Marketing pages can be plain React components with reusable sections.
- Keep product-specific copy isolated in config/content files where possible.
- Pricing must use the billing pricing endpoint rather than hardcoded plan prices.

## Acceptance criteria
- Public routes work without auth.
- Dashboard routes redirect unauthenticated users.
- Admin routes reject non-admin users.
- Nav reflects logged-in state and subscription status.
- Starter branding can be changed through app config and assets, not by editing many components.
