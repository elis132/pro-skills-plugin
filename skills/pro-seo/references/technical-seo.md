# Technical SEO

Technical SEO ensures Google can find, crawl, understand, and render your content. Without it, even the best content won't rank.

## URL Structure

### Rules

- **Short and descriptive:** `example.com/plumbing-services/` not `example.com/services/category/plumbing-and-related-work/`
- **Use hyphens, not underscores:** `drain-cleaning` not `drain_cleaning`
- **Lowercase only:** `example.com/Services/` = wrong
- **No parameters when possible:** `example.com/plumbing/drain-cleaning/` not `example.com/services?category=plumbing&type=drain-cleaning`
- **Include target keyword:** `example.com/emergency-plumber-denver/` not `example.com/service-page-4/`
- **Consistent trailing slashes:** Pick one format and stick with it

### URL Structure Example

```
example.com/                                 (homepage)
example.com/services/                        (services overview)
example.com/services/plumbing/               (plumbing pillar)
example.com/services/plumbing/emergency/     (emergency plumbing)
example.com/services/plumbing/drain-cleaning/ (drain cleaning)
example.com/services/plumbing/water-heater/  (water heater)
example.com/blog/                            (blog index)
example.com/blog/plumbing-cost-denver/       (blog post)
example.com/blog/how-to-unclog-drain/        (blog post)
example.com/areas/denver/                    (location page)
example.com/areas/aurora/                    (location page)
example.com/about/                           (about page)
example.com/contact/                         (contact page)
```

### URL Changes

**Never change URLs without 301 redirects.** Every changed URL without a redirect loses all its accumulated authority and creates a 404 error.

```
Old: example.com/services/plumbing-repair/
New: example.com/services/plumbing/repair/
→ 301 redirect from old to new
```

## Canonical Tags

Canonical tags tell Google which version of a page is the "real" one. Essential for avoiding duplicate content issues.

### When to Use

- **Always** have a self-referencing canonical on every page
- **URL parameters:** If `example.com/services/` and `example.com/services/?ref=google` both exist, canonical should point to the clean URL
- **HTTP vs. HTTPS:** Canonical should always point to HTTPS
- **WWW vs. non-WWW:** Pick one, canonical the other
- **Pagination:** Page 2, 3, etc. should canonical to themselves (not page 1)

### Implementation

```html
<link rel="canonical" href="https://example.com/services/plumbing/" />
```

**Every page** should have this in the `<head>`. No exceptions.

## Core Web Vitals

Google's page experience signals. Measured on real user data (Chrome User Experience Report).

### The Three Metrics

| Metric | What It Measures | Target | Common Fix |
|--------|-----------------|--------|------------|
| **LCP** (Largest Contentful Paint) | Loading speed | < 2.5s | Optimize images, server response time |
| **INP** (Interaction to Next Paint) | Interactivity | < 200ms | Reduce JavaScript, optimize event handlers |
| **CLS** (Cumulative Layout Shift) | Visual stability | < 0.1 | Set image dimensions, avoid dynamic content injection |

### Quick Wins

1. **Images:** Compress all images to WebP. Use `width` and `height` attributes. Lazy load below-the-fold images.
2. **Server:** Use a CDN. Enable compression (gzip/Brotli). Optimize server response time (< 200ms).
3. **JavaScript:** Defer non-critical JS. Remove unused JS. Don't use render-blocking scripts.
4. **CSS:** Inline critical CSS. Defer non-critical CSS. Remove unused CSS.
5. **Fonts:** Use `font-display: swap`. Preload critical fonts. Limit font variations.

### Testing Tools

- **Google PageSpeed Insights:** Real user data + lab data
- **Google Search Console:** Core Web Vitals report
- **Chrome DevTools → Lighthouse:** Lab testing
- **WebPageTest.org:** Detailed waterfall analysis

## Crawlability

If Google can't crawl your pages, they can't rank.

### robots.txt

Controls which pages Google is allowed to crawl.

```
# example.com/robots.txt

User-agent: *
Allow: /
Disallow: /admin/
Disallow: /cart/
Disallow: /checkout/
Disallow: /account/
Disallow: /api/
Disallow: /tmp/
Disallow: /*?sort=
Disallow: /*?filter=

Sitemap: https://example.com/sitemap.xml
```

**Common mistakes:**
- Accidentally blocking `/services/` or entire directories
- Blocking CSS/JS files (Google needs them to render pages)
- Forgetting to update after site redesign
- Not including sitemap reference

### XML Sitemap

A roadmap for Google's crawlers.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2025-03-15</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://example.com/services/plumbing/</loc>
    <lastmod>2025-03-10</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.9</priority>
  </url>
  <url>
    <loc>https://example.com/services/plumbing/emergency/</loc>
    <lastmod>2025-03-01</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://example.com/blog/plumbing-cost-denver/</loc>
    <lastmod>2025-02-20</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.6</priority>
  </url>
</urlset>
```

**Rules:**
- Include all pages you want indexed
- Exclude pages you DON'T want indexed (admin, cart, filtered URLs)
- Keep `lastmod` accurate (not just today's date on everything)
- Submit to Google Search Console
- Max 50,000 URLs per sitemap (use sitemap index for larger sites)

### Internal Link Crawlability

Google discovers pages by following links. Orphan pages (no internal links pointing to them) are hard for Google to find, even if they're in the sitemap.

**Every important page should be reachable within 3 clicks from the homepage.**

```
Homepage → Services → Plumbing → Emergency Plumbing (3 clicks)
Homepage → Blog → "Plumbing Cost Guide" (2 clicks)
```

If a page is buried 5+ clicks deep, consider adding a direct link from a higher-level page.

## On-Page SEO Elements

### Title Tag

- 50-60 characters
- Primary keyword near the start
- Unique per page
- Compelling (not just keyword-stuffed)

```html
<title>Emergency Plumber Denver — 24/7 Same-Day Service | Denver Premier Plumbing</title>
```

### Meta Description

- 150-160 characters
- Include primary keyword
- Include CTA or value proposition
- Unique per page

```html
<meta name="description" content="Licensed Denver plumber with 24/7 emergency service. Average response time: 45 minutes. Free estimates on all repairs. Call (303) 555-0147." />
```

### Header Tags (H1-H6)

- One H1 per page (containing primary keyword)
- H2s for main sections
- H3s for subsections
- Don't skip levels (H1 → H3 without H2)
- Use headers for structure, not for styling

### Image Optimization

```html
<img
  src="/images/drain-cleaning-denver.webp"
  alt="Professional plumber using hydro-jetter for drain cleaning in Denver home"
  width="800"
  height="600"
  loading="lazy"
/>
```

- **File name:** Descriptive (`drain-cleaning-denver.webp` not `IMG_4523.jpg`)
- **Alt text:** Descriptive and keyword-relevant
- **Format:** WebP (30-50% smaller than JPEG)
- **Dimensions:** Set width and height to prevent CLS
- **Loading:** Lazy load below-the-fold images

### Open Graph and Twitter Cards

For social sharing. Not a ranking factor, but drives traffic.

```html
<meta property="og:title" content="Emergency Plumber Denver — 24/7 Service" />
<meta property="og:description" content="Licensed plumber with average 45-minute response time." />
<meta property="og:image" content="https://example.com/images/og-emergency-plumber.jpg" />
<meta property="og:url" content="https://example.com/services/plumbing/emergency/" />
<meta property="og:type" content="website" />
<meta name="twitter:card" content="summary_large_image" />
```

## HTTPS

Non-negotiable. Google has confirmed HTTPS is a ranking signal. Any site still on HTTP is at a disadvantage.

**Checklist:**
- SSL certificate installed and valid
- All HTTP URLs redirect to HTTPS (301)
- All internal links use HTTPS
- No mixed content (HTTP resources on HTTPS pages)
- Canonical tags use HTTPS
- Sitemap uses HTTPS URLs

## Mobile Optimization

Google uses mobile-first indexing. The mobile version of your site IS the version Google ranks.

**Checklist:**
- Responsive design (works on all screen sizes)
- Text readable without zooming (min 16px font)
- Buttons large enough to tap (min 48px × 48px)
- No horizontal scrolling
- Content identical to desktop version (not a stripped-down mobile version)
- Fast loading on mobile networks (test with 3G throttling)

## Redirects

### Types

| Type | When to Use |
|------|-------------|
| **301 (Permanent)** | Page permanently moved. Passes ~90% of link authority. |
| **302 (Temporary)** | Page temporarily moved. Does NOT pass authority. |
| **Meta refresh** | Never use for SEO purposes. |
| **JavaScript redirect** | Avoid. Google may not follow. |

### Common Redirect Scenarios

```
# Page moved
/old-service/ → /services/plumbing/ (301)

# Domain change
oldsite.com/* → newsite.com/* (301, maintain path structure)

# HTTP to HTTPS
http://example.com/* → https://example.com/* (301)

# WWW to non-WWW (or vice versa)
www.example.com/* → example.com/* (301)

# Trailing slash normalization
/services → /services/ (301)
```

### Redirect Chains

Avoid chains: A → B → C → D. Each redirect loses a small amount of authority and adds latency. Maximum acceptable chain length: 2 hops. Ideal: 1 hop (direct).

## Structured Data Testing

Always validate structured data before deploying:

1. **Google Rich Results Test:** https://search.google.com/test/rich-results
2. **Schema.org Validator:** https://validator.schema.org/
3. **Google Search Console → Enhancements:** Monitor for errors

## Technical SEO Audit Checklist

Run through this checklist for any new site or after major changes:

- [ ] All pages return 200 status code (or appropriate redirect)
- [ ] No broken links (404s)
- [ ] robots.txt is correct and not blocking important pages
- [ ] XML sitemap exists and is submitted to Search Console
- [ ] All pages have unique title tags
- [ ] All pages have unique meta descriptions
- [ ] All pages have canonical tags
- [ ] HTTPS is enforced with 301 redirects
- [ ] Mobile-friendly (responsive design)
- [ ] Core Web Vitals pass (LCP < 2.5s, INP < 200ms, CLS < 0.1)
- [ ] Images are optimized (WebP, compressed, have alt text)
- [ ] No orphan pages
- [ ] No redirect chains longer than 2 hops
- [ ] Schema markup validated and error-free
- [ ] No duplicate content issues
- [ ] Google Search Console connected and monitored
- [ ] 404 page exists and is helpful (includes navigation, search)
