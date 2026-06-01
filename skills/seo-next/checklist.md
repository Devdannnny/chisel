# SEO Audit Rubric — 100 points

Score each line **pass / fail / N-A**. A domain is green only when all non-N-A items pass. Record evidence (file:line or measured value) for every pass — a checked box with no evidence does not count. Total the weighted domains for the final score.

For each item: the Next.js mechanism that satisfies it is in parentheses.

---

## Meta Tags — 15 pts

- [ ] Unique `<title>`, 30–60 chars, primary keyword near the front (`metadata.title` / `generateMetadata`)
- [ ] Unique meta description, 120–160 chars, benefit-led (`metadata.description`)
- [ ] Title template applied sitewide so every page reads `Page · Brand` (`metadata.title.template` in root layout)
- [ ] Self-referencing canonical on every indexable route (`alternates.canonical` + `metadataBase`)
- [ ] Viewport meta present and correct (Next emits by default; don't override badly)
- [ ] Robots directives correct per route — `index/follow` for public, `noindex` for thin/utility pages (`metadata.robots`)
- [ ] `metadataBase` set so all relative metadata URLs resolve to the production origin
- [ ] No duplicate titles/descriptions across routes

## Technical SEO — 15 pts

- [ ] `robots.txt` served and correct (`app/robots.ts`)
- [ ] `sitemap.xml` served, complete, and only lists indexable URLs (`app/sitemap.ts`)
- [ ] robots ⇄ sitemap consistent — nothing `disallow`ed or `noindex`ed appears in the sitemap
- [ ] Clean, lowercase, hyphenated URLs; no query-string-only routing for content
- [ ] Correct status codes — 200 for live, 301 for moved (`redirects()` in `next.config`), 404 returns 404 (`not-found.tsx`)
- [ ] No redirect chains or loops
- [ ] `lang` attribute on `<html>`; hreflang/`alternates.languages` if multi-locale (i18n routing)
- [ ] Trailing-slash behavior consistent (one canonical form)

## Core Web Vitals — 15 pts (measured on deployed URL)

- [ ] LCP < 2.5s (`priority` image, `next/font`, Suspense streaming)
- [ ] CLS < 0.1 (image dimensions, reserved space, no FOUT)
- [ ] INP < 200ms (small client bundles, deferred JS)
- [ ] FCP healthy (SSR/SSG, no render-blocking resources)
- [ ] TTFB low (Server Components, static/ISR, edge caching)
- [ ] No render-blocking third-party scripts (`next/script` strategy)

## Security — 10 pts

- [ ] HTTPS enforced
- [ ] HSTS header (`Strict-Transport-Security` in `next.config` headers)
- [ ] Content-Security-Policy set
- [ ] `X-Content-Type-Options: nosniff`, `Referrer-Policy` set
- [ ] No secrets/keys exposed in client bundles (`NEXT_PUBLIC_` audited)

## Links — 10 pts

- [ ] No broken internal or outbound links (crawl/check)
- [ ] Descriptive anchor text — no "click here"
- [ ] Internal links use `next/link` (prefetch, client nav)
- [ ] Important pages reachable within ~3 clicks of home
- [ ] External links use `rel` appropriately (`noopener`; `nofollow`/`sponsored` where warranted)

## Images — 10 pts

- [ ] All meaningful images use `next/image` (not raw `<img>`)
- [ ] Every image has meaningful `alt` (empty `alt=""` only for decorative)
- [ ] Explicit `width`/`height` or `fill` + sized container (no CLS)
- [ ] Modern formats served (AVIF/WebP via `next/image` defaults)
- [ ] Above-the-fold hero uses `priority`; below-fold lazy-loads by default

## Headings — 10 pts

- [ ] Exactly one `<h1>` per page, containing the primary keyword
- [ ] Logical hierarchy, no skipped levels (h1→h2→h3)
- [ ] Headings describe content, not used for styling
- [ ] Heading text is unique and descriptive

## Structured Data — 8 pts

- [ ] Sitewide `Organization` + `WebSite` JSON-LD in root layout
- [ ] Page-type schema where applicable (`Article`/`BlogPosting`, `Product`+`Offer`, `FAQPage`)
- [ ] `BreadcrumbList` on nested routes
- [ ] All JSON-LD validates in Google Rich Results Test
- [ ] Schema accurately reflects real page content (no spam/fake reviews)

## Social — 7 pts

- [ ] Open Graph tags complete (`og:title`, `og:description`, `og:type`, `og:url`, `og:image`) via `metadata.openGraph`
- [ ] Twitter card set (`summary_large_image`) via `metadata.twitter`
- [ ] Per-route OG image, 1200×630 (`opengraph-image.tsx` or static asset)
- [ ] OG/Twitter image URLs absolute (resolved via `metadataBase`)

---

## Score tally

| Domain | Weight | Earned |
|---|---:|---:|
| Meta Tags | 15 | |
| Technical SEO | 15 | |
| Core Web Vitals | 15 | |
| Security | 10 | |
| Links | 10 | |
| Images | 10 | |
| Headings | 10 | |
| Structured Data | 8 | |
| Social | 7 | |
| **Total** | **100** | |
