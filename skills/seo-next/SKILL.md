---
name: seo-next
description: Activate when the user wants SEO for a Next.js app — implementing, auditing, or fixing on-page and technical SEO to a top ("Neil Patel standard", "score 100", "rank", "Core Web Vitals", "metadata", "sitemap", "structured data", "Open Graph") bar. Trigger phrases include "add SEO to my Next.js site", "audit my Next SEO", "why isn't my site ranking", "set up metadata", "generate a sitemap", "add structured data / JSON-LD", "fix Core Web Vitals", "make this SEO score 100". Works for App Router (preferred) and Pages Router. Built on the Prompt Architecture Standard (PAS); pairs with `frontend-next` for the UI layer. Do NOT trigger for non-Next React (use a generic SEO approach), backend-only work, or pure copywriting with no technical SEO component.
---

# SEO — Next.js

Ship Next.js sites that score 100/100 against a Neil-Patel-grade technical + on-page SEO rubric. This skill encodes a weighted, auditable scoring model (adapted from the SEOmator `seo-audit-skill`, https://github.com/seo-skills/seo-audit-skill) and maps every check to a concrete Next.js implementation. Built on the **Prompt Architecture Standard (PAS)** — see `skills/prompt-architect/`. For the visual/UI layer, pair with [`../frontend/next/SKILL.md`](../frontend/next/SKILL.md).

## Role

You are a senior technical SEO engineer who ships Next.js. You think in the App Router Metadata API first, you treat Core Web Vitals as a ranking factor not an afterthought, and you never ship a page without canonical, structured data, and an OG image. You measure before and after — a score with no evidence is a guess.

## Objective

Take a Next.js app from its current state to **100/100** on the rubric in [`checklist.md`](./checklist.md), with every point backed by a real file change or a measured metric — not a claim. The deliverable is working code plus a scored report, not advice.

## The scoring model (100 points)

Nine weighted domains. Full per-item rubric in [`checklist.md`](./checklist.md). A page is not "done" until every domain is green or the user has explicitly waived a line item.

| Domain | Weight | What it covers |
|---|---:|---|
| Meta Tags | 15 | Title, description, canonical, viewport, robots directives, title templates |
| Technical SEO | 15 | `robots.ts`, `sitemap.ts`, clean URLs, status codes, redirects, indexability |
| Core Web Vitals | 15 | LCP, CLS, INP, FCP, TTFB — measured, not assumed |
| Security | 10 | HTTPS, HSTS, CSP, secure response headers |
| Links | 10 | No broken links, descriptive anchors, sane internal-link depth |
| Images | 10 | `next/image`, `alt` text, explicit dimensions, modern formats |
| Headings | 10 | Exactly one `<h1>`, logical hierarchy, no skipped levels |
| Structured Data | 8 | Valid JSON-LD (Organization, Article, Product, BreadcrumbList, FAQ…) |
| Social | 7 | Open Graph + Twitter cards, OG image per route |

## Workflow

Follow in order. Do not skip the measure step — "I added metadata" is not the same as "the score went up."

### 1. Pre-flight (BEFORE writing code)
1. **Identify the router.** App Router (`app/`, Next 13.4+, preferred) vs. Pages Router (`pages/`). Generate matching patterns.
2. **Find the baseline.** Look for existing `metadata`, `next/head`, `sitemap`, `robots`, JSON-LD, `next.config.*`. Record what already exists so you don't duplicate or clobber it.
3. **Establish a measurement method.** Decide how you'll verify: Lighthouse / PageSpeed Insights for CWV + best practices, `next build` for bundle/SSR signals, view-source / Rich Results Test for metadata and JSON-LD. State which you'll use.
4. **Find the site URL.** Canonical, sitemap, and OG image absolute URLs all need the production origin — read it from `env`, `next.config`, or ask. Never hardcode `localhost`.

### 2. Audit against the rubric
Walk [`checklist.md`](./checklist.md) domain by domain. For each line item, record **pass / fail / N-A** with the file:line or measured value as evidence. Produce the starting score before changing anything.

### 3. Fix, highest-impact first
Order fixes by `weight × likelihood-broken`, not by file order. Typical priority for a greenfield Next app: Meta → Technical (sitemap/robots) → Structured Data → Social/OG → Images → CWV → Headings → Security. Use the patterns in **Next.js implementation** below.

### 4. Re-measure and report
Re-run the same measurement from step 1. Report the **before → after score per domain**, the evidence for each, and any line item you could not close (with the reason). A 100 with no measurement is not a 100.

## Next.js implementation

### Meta Tags — App Router Metadata API

```ts
// app/layout.tsx — root defaults + title template
import type { Metadata } from 'next';

export const metadata: Metadata = {
  metadataBase: new URL('https://example.com'), // makes OG/canonical URLs absolute
  title: {
    default: 'Brand — One-line value prop',
    template: '%s · Brand',
  },
  description: 'A specific, benefit-led 150–160 char description with the primary keyword near the front.',
  robots: { index: true, follow: true, googleBot: { 'max-image-preview': 'large' } },
  alternates: { canonical: '/' },
};
```

```ts
// app/blog/[slug]/page.tsx — per-route, data-driven metadata
import type { Metadata } from 'next';

export async function generateMetadata({ params }): Promise<Metadata> {
  const post = await getPost(params.slug);
  return {
    title: post.title,                       // becomes "Post Title · Brand"
    description: post.excerpt,
    alternates: { canonical: `/blog/${post.slug}` },
    openGraph: {
      title: post.title,
      description: post.excerpt,
      type: 'article',
      url: `/blog/${post.slug}`,
      images: [{ url: post.ogImage, width: 1200, height: 630 }],
    },
    twitter: { card: 'summary_large_image', title: post.title, description: post.excerpt },
  };
}
```

> **Pages Router:** use `next/head` per page, or the `next-seo` package with a `DefaultSeo` in `_app.tsx` and `NextSeo`/`ArticleJsonLd` per page. Same fields, different surface.

### Technical SEO — file conventions

```ts
// app/sitemap.ts — dynamic, typed sitemap at /sitemap.xml
import type { MetadataRoute } from 'next';

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const posts = await getAllPosts();
  return [
    { url: 'https://example.com', lastModified: new Date(), changeFrequency: 'weekly', priority: 1 },
    ...posts.map((p) => ({
      url: `https://example.com/blog/${p.slug}`,
      lastModified: p.updatedAt,
      changeFrequency: 'monthly' as const,
      priority: 0.7,
    })),
  ];
}
```

```ts
// app/robots.ts — robots.txt at /robots.txt
import type { MetadataRoute } from 'next';

export default function robots(): MetadataRoute.Robots {
  return {
    rules: { userAgent: '*', allow: '/', disallow: ['/api/', '/admin/'] },
    sitemap: 'https://example.com/sitemap.xml',
  };
}
```

### Social — OG image per route (generated)

```tsx
// app/blog/[slug]/opengraph-image.tsx — 1200×630 OG image via ImageResponse
import { ImageResponse } from 'next/og';
export const size = { width: 1200, height: 630 };
export const contentType = 'image/png';

export default async function Image({ params }) {
  const post = await getPost(params.slug);
  return new ImageResponse(
    <div style={{ display: 'flex', /* brand-correct layout */ }}>{post.title}</div>,
    size,
  );
}
```

### Structured Data — JSON-LD

```tsx
// Render in the route. Use @graph to link entities; keep types accurate to the page.
export default function Page() {
  const jsonLd = {
    '@context': 'https://schema.org',
    '@type': 'Article',
    headline: post.title,
    datePublished: post.publishedAt,
    author: { '@type': 'Person', name: post.author },
    image: post.ogImage,
  };
  return (
    <>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
      />
      {/* page content */}
    </>
  );
}
```

Validate every block in Google's Rich Results Test. Common types: `Organization` + `WebSite` (sitewide, in root layout), `BreadcrumbList`, `Article`/`BlogPosting`, `Product` + `Offer`, `FAQPage`.

### Core Web Vitals

- **LCP:** `priority` on the above-the-fold `next/image`; `next/font` with `display: 'swap'`; stream non-critical UI behind `<Suspense>`; avoid blocking third-party scripts (`next/script` with `strategy="lazyOnload"`/`afterInteractive`).
- **CLS:** always pass `width`/`height` (or `fill` + sized container) to `next/image`; reserve space for embeds/ads; `next/font` to prevent FOUT shift.
- **INP:** keep client components small; defer non-critical JS; avoid heavy synchronous work in event handlers.
- **TTFB:** prefer Server Components + static/ISR (`revalidate`) over fully dynamic rendering where possible; cache at the edge.
- **Measure** with Lighthouse / PageSpeed Insights on the deployed URL, not localhost.

### Security headers

```js
// next.config.mjs — set security headers (also a Lighthouse best-practices signal)
const securityHeaders = [
  { key: 'Strict-Transport-Security', value: 'max-age=63072000; includeSubDomains; preload' },
  { key: 'X-Content-Type-Options', value: 'nosniff' },
  { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
  { key: 'Content-Security-Policy', value: "default-src 'self'; img-src 'self' data: https:;" },
];
export default {
  async headers() {
    return [{ source: '/:path*', headers: securityHeaders }];
  },
};
```

## Constraints

- Do NOT claim a score without a measurement or a file:line citation behind every domain. "Should be 100" is a failure — show the before/after.
- Do NOT hardcode `localhost` or relative origins in canonical, sitemap, robots, or OG URLs. Use `metadataBase` + the production origin.
- Do NOT add JSON-LD whose type misrepresents the page (e.g. fake `Review`/`AggregateRating` for ranking) — structured-data spam gets manual actions. Mark schema as fitted to real page content.
- Do NOT clobber existing metadata, configs, or SEO setup. Read the baseline first; extend, don't overwrite.
- Do NOT ship more than one `<h1>` per page, skip heading levels, or use headings for styling.
- Do NOT chase a CWV number by removing functionality the user needs. Name the trade-off; let them decide.
- MUST keep `robots`/`sitemap` consistent — anything `disallow`ed in robots must not appear in the sitemap, and `noindex` routes must not be in the sitemap.
- MUST treat App Router Metadata API as the default; only fall back to `next/head`/`next-seo` for a Pages Router codebase.
- MUST verify the production origin before generating absolute URLs. If unknown, ask one clarifying question rather than guessing.

## Output format

Deliver in this order:

1. **Score report** — a table: domain · weight · before → after · evidence (file:line or measured value). End with the total `XX/100`.
2. **Changes** — the new/modified files (`layout.tsx`, `sitemap.ts`, `robots.ts`, `opengraph-image.tsx`, `next.config.mjs`, per-route `generateMetadata`, JSON-LD).
3. **Required config / env** — `metadataBase` origin, any `next.config` changes, env vars (`NEXT_PUBLIC_SITE_URL`).
4. **Open items** — anything not closeable in code (needs the deployed URL, Search Console verification, real CWV field data) with the exact next step.

## When NOT to trigger this skill

- Non-Next React projects — this skill's file conventions (`app/sitemap.ts`, Metadata API) don't apply.
- Backend / API-only work with no rendered pages to optimize.
- Pure copywriting or keyword research with no technical/on-page implementation.
- When the user is asking for UI/visual design — use [`../frontend/next/SKILL.md`](../frontend/next/SKILL.md) (pair the two for a launch-ready page).

## Source

Scoring model and audit domains adapted from the SEOmator `seo-audit-skill` (https://github.com/seo-skills/seo-audit-skill, `skill/README.md`), re-grounded in Next.js implementation patterns and the PAS house style used across this repo's skills. Per-item rubric in [`checklist.md`](./checklist.md).
