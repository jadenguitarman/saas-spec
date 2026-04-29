# BLOG_SPEC.md

## Goal
Implement a simple Markdown-based blog/SEO content system like Load Calc Guru's `blog/` plus `lib/blog.ts` pattern.

## Content source
- Store posts as Markdown files in `/blog`.
- One file per slug.
- Use front matter for metadata.

Example:

```md
---
title: "Post title"
description: "SEO description"
date: "2026-01-01"
author: "Team"
published: true
---

Post body.
```

## Required metadata
- `title`
- `description`
- `date`
- `published`

Optional metadata:
- `author`
- `image`
- `tags`
- `canonicalUrl`

## Required routes

```text
app/(marketing)/blog/page.tsx
app/(marketing)/blog/[slug]/page.tsx
app/api/blog-og/route.tsx
```

## Required library

```text
lib/blog.ts
```

Responsibilities:
- Read Markdown posts.
- Parse front matter.
- Generate slugs from filenames.
- Sort posts by date descending.
- Exclude unpublished posts in production.
- Render Markdown to safe HTML or MDX-like output.

## SEO behavior
- Blog index has metadata title/description.
- Post pages generate metadata from front matter.
- Sitemap includes published blog posts.
- OG image route can generate a simple title card.

## Styling behavior
- Use a reusable article layout component.
- Use prose styles from Tailwind typography or equivalent local styling.
- Headings should get stable IDs for anchor links.

## Starter content
Include only neutral placeholder posts or no posts. Do not include Load Calc Guru HVAC articles in the starter.

## Acceptance criteria
- Adding `blog/example.md` creates `/blog/example`.
- Unpublished posts do not render in production.
- Metadata and sitemap include published posts.
- Blog has no dependency on product domain modules.
