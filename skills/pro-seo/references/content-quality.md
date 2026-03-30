# Content Quality

Google's 2025 ranking algorithm boils down to one core question: "Does this content deserve to rank?" Not "is it optimized" — but does it provide genuine, unique value that the searcher can't easily find elsewhere?

## E-E-A-T: Experience, Expertise, Authoritativeness, Trustworthiness

Google's quality framework. Not a ranking factor per se, but the lens through which Google evaluates content quality.

### Experience

Has the author actually DONE the thing they're writing about?

**Signals Google looks for:**
- First-person accounts ("When we replaced a sewer line last month...")
- Original photos (not stock photos)
- Specific details that only come from real experience
- Mentions of real tools, real challenges, real outcomes

**How to demonstrate:**
- Include project photos (before/after)
- Share specific measurements, timelines, and costs from real jobs
- Mention specific equipment used ("We used a 50-foot drain snake with a 3/4-inch head for this job")
- Discuss things that went wrong and how you handled them

**Bad example:**
> "Drain cleaning is an important plumbing service. Professional plumbers use specialized tools to clear your drains."

**Good example:**
> "Last week we responded to a grease blockage in a restaurant kitchen in downtown Denver. The main drain line was 80% blocked — our camera inspection showed years of built-up grease starting about 15 feet in. We used a hydro-jetter at 4,000 PSI to clear it. Total time on site: 3 hours. The owner had been dealing with slow drains for months and tried chemical cleaners without success."

The difference is night and day. The first could be written by anyone (or any AI). The second could only come from someone who actually does this work.

### Expertise

Does the author know the subject deeply?

**How to demonstrate:**
- Detailed technical explanations that go beyond surface level
- Addressing nuances and edge cases
- Citing specific codes, regulations, or industry standards
- Using correct terminology (but explaining it for laypeople)

**For a plumbing company:**
- Reference specific building codes ("Per the 2021 International Plumbing Code, Section 305.4...")
- Explain WHY certain methods are preferred, not just WHAT to do
- Address common misconceptions with detailed corrections

### Authoritativeness

Is this site recognized as an authority on this topic?

**Built through:**
- Consistent content on your core topic over time
- Backlinks from relevant, authoritative sites
- Mentions in local media, industry publications
- Professional credentials and associations displayed

### Trustworthiness

Can the searcher trust this content?

**Signals:**
- Clear business information (name, address, phone, business license number)
- Real reviews and testimonials (linked to Google, Yelp, etc.)
- Insurance certifications, professional licenses displayed
- Transparent pricing (or at least pricing ranges)
- HTTPS, privacy policy, terms of service
- Author bylines with real names and credentials

**Example of trust signals on a service page:**

```
Denver Premier Plumbing LLC
Licensed & Insured | CO License #PLB-2847561
Member: Plumbing-Heating-Cooling Contractors Association
4.9 stars on Google (287 reviews) | A+ BBB Rating
Serving Denver Metro since 2008
```

## Unique Value

The number one reason content fails to rank: it offers nothing that the existing top results don't already provide.

### The Unique Value Test

Before writing ANY piece of content, answer:
1. What can we say that the current top 5 results DON'T say?
2. What unique data, experience, or perspective do we bring?
3. Why would someone prefer THIS article over what already ranks?

If you can't answer these, don't write the content.

### Sources of Unique Value

| Source | Example |
|--------|---------|
| Original data | "We surveyed 200 Denver homeowners about plumbing costs..." |
| Real experience | "In 15 years of plumbing in Denver, the #1 issue we see is..." |
| Local specificity | "Denver's clay soil causes specific pipe problems that other cities don't face..." |
| Proprietary process | "Our 5-point inspection checklist includes items most plumbers skip..." |
| Contrarian take | "Most articles say X, but in our experience, Y is actually better because..." |
| Visual documentation | Original before/after photos, video walkthroughs, diagrams |
| Expert interviews | Quotes from specialists, inspectors, or manufacturers |

## Content Formatting

How content is structured matters as much as what it says.

### Headers (H1, H2, H3)

- **H1:** One per page. Should include your primary keyword naturally.
- **H2:** Main sections. Target secondary keywords and related questions.
- **H3:** Subsections within H2s. Target long-tail variations and PAA questions.

**Example structure for "How Much Does a Plumber Cost in Denver?"**

```
H1: How Much Does a Plumber Cost in Denver? (2025 Price Guide)
  H2: Average Plumbing Costs in Denver
    H3: Emergency Plumbing Rates
    H3: Standard Service Call Fees
    H3: Hourly vs. Flat-Rate Pricing
  H2: Cost by Service Type
    H3: Drain Cleaning Cost
    H3: Water Heater Installation Cost
    H3: Pipe Repair Cost
    H3: Bathroom Remodel Plumbing Cost
  H2: Factors That Affect Plumbing Costs
    H3: Time of Day and Urgency
    H3: Age of Your Home's Plumbing
    H3: Permit Requirements
  H2: How to Save on Plumbing Costs
  H2: Getting a Quote: What to Expect
```

### Lists and Tables

Use them liberally. They improve readability AND increase chances of featured snippets.

**Pricing tables are particularly effective:**

| Service | Average Cost | Range |
|---------|-------------|-------|
| Drain cleaning | $175 | $100-$350 |
| Water heater install | $1,200 | $800-$2,500 |
| Faucet replacement | $250 | $150-$400 |
| Pipe repair (per section) | $350 | $150-$800 |

*Prices based on Denver metro area averages, 2025. Actual costs may vary.*

### Images and Visual Content

- **Always use original images** when possible. Stock photos scream "generic."
- **Alt text matters.** Describe the image AND include relevant keywords naturally.
  - Bad: `alt="plumber"`
  - Good: `alt="Plumber repairing copper pipe under kitchen sink in Denver home"`
- **Compress images.** Use WebP format. Aim for under 100KB per image.
- **Before/after photos** are SEO gold for service businesses.

### Content Length

There is no magic word count. The right length is: as long as it takes to fully answer the searcher's question, and not a word more.

**Guidelines by intent:**
- Transactional (service pages): 800-1,500 words
- Commercial (comparison/pricing): 1,500-2,500 words
- Informational (how-to guides): 2,000-4,000 words
- Pillar pages: 3,000-5,000 words

These are guidelines, not rules. A page about "emergency plumber Denver" doesn't need 3,000 words. The searcher needs a phone number, service area, pricing, and social proof.

## Schema Markup

Structured data helps Google understand your content. Not a direct ranking factor, but improves how your page appears in search results (rich snippets).

### Essential Schema for Service Businesses

**LocalBusiness schema:**
```json
{
  "@context": "https://schema.org",
  "@type": "Plumber",
  "name": "Denver Premier Plumbing",
  "image": "https://example.com/images/team-photo.jpg",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "1234 Main Street",
    "addressLocality": "Denver",
    "addressRegion": "CO",
    "postalCode": "80202"
  },
  "telephone": "+1-303-555-0147",
  "url": "https://example.com",
  "priceRange": "$$",
  "openingHours": "Mo-Fr 07:00-18:00, Sa 08:00-14:00",
  "areaServed": ["Denver", "Aurora", "Lakewood", "Littleton", "Arvada"],
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.9",
    "reviewCount": "287"
  }
}
```

**FAQ schema (for FAQ sections):**
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "How much does an emergency plumber cost in Denver?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Emergency plumbing in Denver typically costs $150-$300 for the service call, plus hourly rates of $100-$200. After-hours and weekend calls usually carry a premium of $50-$100."
      }
    }
  ]
}
```

**Service schema:**
```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "serviceType": "Emergency Plumbing",
  "provider": {
    "@type": "Plumber",
    "name": "Denver Premier Plumbing"
  },
  "areaServed": {
    "@type": "City",
    "name": "Denver"
  },
  "description": "24/7 emergency plumbing service in Denver. Burst pipes, sewer backups, gas leaks, and flooding. Average response time: 45 minutes."
}
```

### Schema for Content Pages

**Article schema** for blog posts. **HowTo schema** for guides. **Review schema** for testimonial pages. Always validate with Google's Rich Results Test before publishing.

## Meta Descriptions and Title Tags

### Title Tags

The most important on-page SEO element. Shows in search results and browser tabs.

**Formula:** [Primary Keyword] — [Value Proposition] | [Brand]

**Examples:**
- "Emergency Plumber Denver — 24/7 Same-Day Service | Denver Premier Plumbing"
- "How Much Does a Plumber Cost in Denver? (2025 Price Guide)"
- "Drain Cleaning Denver — $99 Camera Inspection Included"

**Rules:**
- 50-60 characters (Google truncates longer titles)
- Primary keyword as close to the beginning as possible
- Include a compelling reason to click (price, speed, unique offer)
- Unique for EVERY page (never duplicate title tags)

### Meta Descriptions

Don't directly affect rankings, but critically affect click-through rate.

**Formula:** [What the page offers] + [Why choose us/this content] + [CTA]

**Examples:**
- "Licensed Denver plumber with 24/7 emergency service. Average response time: 45 minutes. Free estimates on all repairs. Call (303) 555-0147."
- "Complete 2025 guide to plumbing costs in Denver. Average prices for 15+ common services, tips to save money, and how to spot overcharging."

**Rules:**
- 150-160 characters
- Include primary keyword naturally
- Include a call to action or value proposition
- Don't just describe the page — SELL the click
- Unique for EVERY page

## Content Refresh Strategy

Updating existing content often produces faster results than creating new content.

### What to Update

1. **Outdated information** — prices, statistics, regulations
2. **Missing sections** — topics the current top results cover that you don't
3. **Thin sections** — expand areas that are too brief
4. **New internal links** — link to content that didn't exist when you first published
5. **Broken links** — fix or replace them
6. **Images** — add original images, update old ones
7. **Schema markup** — add or update structured data

### When to Update

- Page has dropped 5+ positions for its target keyword
- It's been 12+ months since the last update
- A competitor published a significantly better page on the same topic
- Industry changes have made information outdated
- You have new data, case studies, or photos to add
