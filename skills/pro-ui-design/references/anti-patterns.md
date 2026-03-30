# Anti-Patterns: Defaults Are the Enemy, Not Specific Properties

## The Real Problem

The problem was never gradients, rounded corners, or shadows. The problem is DEFAULTING — applying patterns without thinking about whether they serve this specific design.

A gradient can be beautiful. A rounded corner can be perfect. A shadow can create exactly the right depth. These become anti-patterns only when they're reflexive — when they're there because "that's what websites look like" rather than because a designer decided they were right for THIS thing.

This file lists patterns that are COMMONLY applied without thought. If you're using one, make sure you can explain WHY it's the right choice here. If you can't — that's the signal to reconsider.

## Patterns That Require Justification

### The SaaS Hero Formula
Logo + nav right, big headline, subtext, two buttons, angled product screenshot. If your hero matches this layout, ask: is this structure genuinely serving the content, or am I defaulting to it because it's the first layout that comes to mind? Sometimes it IS the right structure — but you need to have considered alternatives first.

**Alternatives to consider:** Typography-only hero, single image, video, split-screen, start with content (skip the hero), horizontal scroll, interactive element, just a search bar.

### Three Equal Columns
Three evenly-spaced items in a row. This is often the right choice for genuinely equal-weight content. But ask: ARE these three things actually equal in importance? Usually one matters more — and the layout should reflect that.

**If they're truly equal:** Consider staggering, overlapping, a numbered list, or a different visual treatment for each.
**If one matters more:** Give it 2x the space. Make the others subordinate.

### Uniform Card Styling
Every card: same border-radius, same shadow, same padding, same structure. Ask: do these cards contain the same TYPE of information at the same LEVEL of importance? If not, they shouldn't look the same.

### The Divider Pattern
Horizontal rule between every section, or a thin gold line, or consistent spacing. This creates rhythm — which is good. But if EVERY section transition is the same, you've created monotony. Vary your transitions: some hard cuts, some overlaps, some color changes, some with no divider at all.

### Monochrome + Single Accent
90% neutral, one pop of color for CTAs. This is a legitimate, sophisticated strategy. It's also become the default "I want to look designed" choice. If you're reaching for this: is monochrome serving the brand/mood, or is it just the safe option? Some things SHOULD be colorful. A children's brand in monochrome with one accent color is wrong. A party planner's site in muted tones is wrong. Match the energy.

### Scroll Reveal Animations
`opacity: 0 → 1, translateY(20px) → 0` on scroll. This was fresh in 2018. Now it's the new "I added animation" default. Ask:
- Does this page NEED entrance animations? (A tool or dashboard probably doesn't)
- If yes, are they all the same? (Vary direction, timing, or use different effects for different content types)
- Have you considered NO animation? Static pages can feel confident.
- Have you considered MORE animation? Some pages should feel alive and dynamic.

### The "Tasteful Restraint" Trap
This is the most dangerous anti-pattern because it FEELS like good design. Symptoms:
- Muted/desaturated everything
- Serif headlines on a light warm background
- Generous whitespace everywhere
- Minimal color, minimal decoration, minimal personality
- Every section feels "calm" and "refined"

This is appropriate for maybe 20% of projects (luxury, editorial, architecture). For the other 80%, it's a cop-out. A startup landing page should have ENERGY. A game studio's site should be FUN. A food brand should make you HUNGRY. Restraint is one tool — not the only tool.

### Same Animation Everywhere  
Same easing curve, same duration, same direction for every animated element. Real motion design has variety:
- Important elements enter differently from supporting elements
- Some things are fast, some slow
- Some slide, some scale, some fade, some clip-reveal
- Some don't animate at all

## Structural Patterns to Question

### Top-to-Bottom Single-Page Narrative
Hero → section → section → section → CTA → footer. This is ONE way to structure a page. Others:
- **Horizontal scroll** (for portfolios, timelines, stories)
- **Grid/mosaic homepage** (everything visible at once, click to explore)
- **App-like interface** (tabs, sidebar, panels)
- **Split screen** (two persistent halves)
- **Single fold** (everything above the fold, no scrolling)
- **Infinite scroll** (content-driven, social-media-like)
- **Hub and spoke** (minimal homepage, rich subpages)

### Identical Mobile Treatment
"Stack the columns, reduce font sizes, add a hamburger menu." This is functional but lazy. A designed mobile experience might:
- Completely reorganize content priority
- Use horizontal swipe for content that was in columns
- Show/hide different content than desktop
- Use different typography scales (not just smaller versions of desktop)
- Have its own navigation pattern

## The Meta-Pattern: Same Designer Syndrome

If your last 3 designs share more than 2 of these, you're in a rut:
- Same color temperature (all warm, or all cool, or all monochrome)
- Same type strategy (always serif display + sans body, or always geometric sans)
- Same layout structure (always single-column narrative)
- Same spacing feel (always generous whitespace)
- Same animation approach (always subtle scroll reveals)
- Same mood (always "refined and restrained")
- Same section transitions (always ruled lines or dividers)

Catch yourself. Deliberately break the pattern. If your last design was dark/moody/serif, make the next one bright/energetic/sans-serif. Not because it's always better — but because range proves intention.

## The Test

For every design choice, you should be able to complete this sentence:

"I chose [X] because [specific reason related to THIS project's audience/mood/brand/context]."

If the best you can do is "I chose [X] because it looks good" or "I chose [X] because it's professional" — that's a default, not a decision.
