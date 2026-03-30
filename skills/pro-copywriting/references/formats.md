# Format-Specific Guidance

## Landing Page Copy

### Hero Section
The hero has 3-5 seconds. Rules:
- **Headline:** One claim. Specific. Under 10 words ideal. States the benefit or makes a promise. NOT what the product IS — what it DOES for the reader.
- **Subheadline:** Expands on HOW. One sentence. Include a specific detail.
- **CTA:** Action verb + what happens. Not "Get Started" — "Start monitoring free" or "Book a tasting."

❌ Headline: "The Next Generation of API Monitoring"
✅ Headline: "Know it's down before they do"

❌ Subheadline: "A comprehensive platform for all your monitoring needs"
✅ Subheadline: "30-second checks from 12 regions. One curl command to set up."

### Feature Descriptions
Don't describe what the feature IS. Describe what it SOLVES.

❌ "Real-time alerting system with multi-channel support"
✅ "Alert hits your phone in 45 seconds. Slack, PagerDuty, SMS — wherever you actually look at 3am."

❌ "Custom design consultation"
✅ "We sit down and taste our way to your flavor. Most couples change their mind at least once. That's the whole point."

### Social Proof
One powerful quote > three generic ones. The quote should be specific:

❌ "Great product! Highly recommend." — John D., CEO
✅ "We replaced Datadog, Sentry, and a custom tracing system. Observability bill dropped 60%." — Platform lead, Series C fintech

If you can't get specific quotes, use metrics: "340 teams. 2.4B spans/day. 18ms median ingest latency."

### CTA Sections
The CTA should reduce anxiety, not create pressure:
- Address the #1 objection ("Free up to 100M spans. No credit card.")
- Make the next step small ("Book a 15-minute tasting. No commitment.")
- Be specific about what happens next ("You'll get access in 30 seconds.")

## Email Copy

### Cold Outreach
Nobody reads long cold emails. Rules:
- First sentence: why you're emailing THIS person (specific reference to their work/company)
- Second sentence: what you have / what you want (one sentence, specific)
- Third sentence: the ask (one small, clear action)
- That's it. 3-5 sentences total.

❌ "I hope this email finds you well. My name is... and I'm reaching out because..."
✅ "Saw your post about the Riverside HOA procurement process — we had a similar experience with [municipality]. Quick question about your approach to the appeal."

### Transactional / Customer Emails
Write like a human colleague, not a system:

❌ "Your order has been successfully processed. You will receive a confirmation email shortly."
✅ "Order confirmed — your monitoring for api.example.com starts in about 30 seconds."

❌ "We regret to inform you that your account has been suspended due to a billing issue."
✅ "Your payment didn't go through, so monitoring is paused. Update your card and it'll restart automatically: [link]"

### Newsletter / Updates
Open with the ONE most interesting thing. Not a summary. Not "In this month's update..." The most interesting thing.

## Product Descriptions

### The Formula
What it is (one line) → Who it's for (implied, not stated) → The specific thing that makes it different → Proof

"Three-tier buttercream cake with seasonal berries and edible flowers. Every tier is a different flavor — because your guests deserve more than vanilla three times. Designed and baked in our downtown studio. Serves 80-100."

Not: "This beautiful three-tier cake is perfect for weddings and special celebrations. Made with high-quality ingredients, it features a stunning design that will impress your guests."

### UI Copy / Microcopy

UI text should be:
- **Helpful** (tells you what to do, not what happened)
- **Specific** (not "An error occurred" — "Couldn't reach the server. Check your connection.")
- **Brief** (UI isn't a place for prose)
- **Human** (occasional personality in empty states, 404 pages, loading states)

Error messages:
❌ "An unexpected error has occurred. Please try again later."
✅ "That didn't work — the payment gateway timed out. Try again in a minute, or switch to card."

Empty states:
❌ "No data available."
✅ "No incidents this week. (That's good.)"

Loading states:
❌ "Loading..."
✅ "Checking 12 regions..." or "Warming up..." or just a well-designed spinner with no text.

## Blog Posts / Articles

### Opening
The first paragraph is everything. Rules:
- No "In today's..." No "Have you ever wondered..." No "When it comes to..."
- Start with: a bold claim, a specific anecdote, a surprising fact, a question that creates tension
- The reader decides in 2 sentences whether to keep reading

### Structure
Don't tell the reader what you'll cover ("In this article, we'll explore..."). Just cover it.

Subheadings should be interesting enough to read on their own:
❌ "Benefits of Monitoring"
✅ "What happens at 3am when nobody's watching"

❌ "Our Process"
✅ "Three iterations for one flavor"

### Ending
Don't summarize. Don't "In conclusion..." End with:
- A callback to the opening
- A forward-looking statement
- A challenge to the reader
- The most important point, restated boldly

## Pitch Deck Copy

Every slide: ONE idea. 10-15 words max for the headline. Then shut up — the presenter fills in the rest.

❌ Slide headline: "Our Comprehensive Solution Addresses Key Pain Points in the Market"
✅ Slide headline: "3am. Your server is down. You don't know yet."

Numbers on slides should be BIG and ALONE:
❌ "We've achieved impressive growth with over 340 customers across multiple sectors"
✅ "340" (with "engineering teams" in small text below)

## Case Studies

Structure: Situation → Tension → Resolution → Result

Not: "Challenge → Solution → Results" (the AI default that reads like a form)

Write it like a STORY:
"When [Company] migrated to Kubernetes, their alert volume tripled overnight. Most were false positives — but buried in the noise were real incidents that took 3x longer to find. They needed a way to separate signal from noise without rebuilding their entire monitoring stack.

They configured Pulseline on a Tuesday. By Thursday, false alerts were down 94%. The team that used to dread on-call started volunteering for it."

Not: "The client faced challenges with their monitoring solution. After implementing our platform, they experienced significant improvements in alert accuracy and team satisfaction."
