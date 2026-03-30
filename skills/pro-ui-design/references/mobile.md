# Mobile Design: Not Just "Stack and Shrink"

## The Problem

95% of AI-generated responsive design does exactly this:
1. Take the desktop layout
2. Stack columns vertically
3. Reduce font sizes proportionally
4. Add a hamburger menu
5. Call it done

This produces a FUNCTIONAL mobile experience. It does not produce a DESIGNED mobile experience. A senior designer treats mobile as its own canvas with its own design decisions.

## Philosophy

Mobile is not a degraded desktop. It's a different context entirely:
- **Physical:** Held in one hand, viewed 30cm from face, operated by thumb
- **Attentional:** Often used while doing something else (commuting, waiting, walking)
- **Emotional:** More intimate — it's YOUR device, in YOUR hand
- **Technical:** Smaller viewport, touch input, variable network speed

These differences should drive different design decisions, not just different column counts.

## Responsive Strategy

### Breakpoints as Design Decisions

Don't use breakpoints to "fix" the desktop layout. Use them to REDESIGN:

```css
/* Mobile-first: design for mobile, THEN enhance */
/* Base styles ARE the mobile styles */

.hero-title {
  font-size: 2.75rem;  /* Designed for mobile, not "reduced from desktop" */
  line-height: 1.05;
}

/* Tablet — not just "bigger phone" */
@media (min-width: 768px) {
  .hero-title {
    font-size: 4rem;
    /* Maybe different alignment or layout here */
  }
}

/* Desktop — the enhancement, not the starting point */
@media (min-width: 1024px) {
  .hero-title {
    font-size: 6rem;
    letter-spacing: -0.03em;  /* Tighter tracking at large sizes */
  }
}
```

### Typography Rescaling

Don't just make everything smaller. The RATIO between sizes should change:

**Desktop:** Hero 88px, body 17px (ratio: 5.2x)
**Mobile:** Hero 44px, body 16px (ratio: 2.75x)

On mobile, the contrast between heading and body DECREASES because the screen is smaller and closer. Extreme size contrast that works at arm's length becomes jarring in your hand.

```css
:root {
  /* Mobile scale */
  --text-body: 1rem;
  --text-lead: 1.125rem;
  --text-title: 1.75rem;
  --text-display: 2.75rem;
}

@media (min-width: 1024px) {
  :root {
    /* Desktop scale — wider range */
    --text-body: 1.0625rem;
    --text-lead: 1.3125rem;
    --text-title: 2.75rem;
    --text-display: 5.5rem;
  }
}
```

### Content Priority

Not everything that's on desktop should be on mobile. Ask:
- What's the ONE thing a mobile user needs? Lead with that.
- What's secondary? Make it accessible but not prominent.
- What's desktop-only? Hide it. `display: none` is a valid design decision.

```css
/* Detailed stats table — great on desktop, unusable on mobile */
.stats-detail { display: none; }
.stats-summary { display: block; } /* Show summary on mobile instead */

@media (min-width: 768px) {
  .stats-detail { display: grid; }
  .stats-summary { display: none; }
}
```

### Navigation

**Hamburger menu is fine.** Stop overthinking it — hamburger menus are a solved pattern that users understand. BUT the menu ITSELF should be designed:

```css
/* Full-screen overlay menu — not a tiny dropdown */
.mobile-menu {
  position: fixed;
  inset: 0;
  background: var(--bg);
  z-index: 100;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 2rem;
}

.mobile-menu a {
  font-size: 2rem;  /* BIG touch targets, BIG text */
  /* This is its own designed moment, not an afterthought */
}
```

Alternative mobile nav patterns:
- **Bottom bar** (like native apps — great for frequently accessed sections)
- **Tab bar** (for apps/dashboards)
- **Scrollable pill bar** (for categories/filters)
- **No nav** (for single-purpose pages — the CTA IS the navigation)

## Touch Design

### Touch Targets
**Minimum 44x44px.** This is not a suggestion — it's the difference between usable and frustrating. Common failures:
- Navigation links with only text height as the target
- Close buttons that are 24px
- Links in dense text paragraphs

```css
/* Invisible padding to increase touch target */
.nav-link {
  padding: 12px 16px;  /* Makes the target large even if the text is small */
  margin: -12px -16px;  /* Optionally counteract the visual padding */
}
```

### Touch-Specific Interactions
Hover states DON'T EXIST on mobile. Design for:
- **Tap states** (`:active` — brief visual feedback)
- **Long-press** (where appropriate)
- **Swipe gestures** (horizontal scroll, cards, carousels)

```css
/* Active state for touch feedback */
.card:active {
  transform: scale(0.98);
  transition: transform 0.1s;
}

/* Remove hover effects on touch devices */
@media (hover: none) {
  .card:hover { transform: none; }
}
```

### Scrolling as Interaction
Mobile users scroll naturally. Use it:
- **Horizontal scroll** for cards, images, options (with `scroll-snap`)
- **Pull-to-refresh patterns** where appropriate
- **Sticky headers** that compress on scroll

```css
/* Horizontal scroll gallery — native-feeling on mobile */
.gallery-scroll {
  display: flex;
  gap: 1rem;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
  padding: 0 1rem;
}

.gallery-scroll > * {
  flex: 0 0 85vw;  /* Cards are 85% of viewport width */
  scroll-snap-align: center;
}

/* Hide scrollbar but keep functionality */
.gallery-scroll::-webkit-scrollbar { display: none; }
.gallery-scroll { scrollbar-width: none; }
```

## Layout Patterns for Mobile

### Instead of stacking columns:

**Horizontal scroll for equal items:**
```css
/* Desktop: 3-column grid */
.features { display: grid; grid-template-columns: repeat(3, 1fr); }

/* Mobile: horizontal scroll, not stacked */
@media (max-width: 768px) {
  .features {
    display: flex;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
  }
  .feature-item {
    flex: 0 0 80vw;
    scroll-snap-align: start;
  }
}
```

**Accordion/collapse for detailed content:**
```css
/* Desktop: side-by-side comparison */
/* Mobile: tappable sections that expand */
```

**Tab interface for categorized content:**
```css
/* Desktop: bento grid showing everything */
/* Mobile: tab bar at top, one category at a time */
```

**Cards with key info + expand for details:**
```css
/* Desktop: full card with all info visible */
/* Mobile: compact card, tap to see full details */
```

## Performance on Mobile

Design decisions that affect mobile performance:
- **Font loading:** Use `font-display: swap` to prevent invisible text. Consider fewer font weights on mobile.
- **Images:** Use `loading="lazy"` for below-fold images. Serve smaller images via `srcset`.
- **Animation:** Reduce or eliminate scroll-triggered animations on mobile — they can cause jank on lower-end devices.

```css
/* Simpler animations on mobile */
@media (max-width: 768px) {
  .reveal {
    /* Skip the transform, just fade */
    opacity: 0;
    transition: opacity 0.5s;
    transform: none;
  }
  .reveal.visible { opacity: 1; }
}
```

## The Mobile Design Checklist

Before shipping, test on a real phone (not just browser devtools):

- [ ] Can you complete the PRIMARY action with your thumb only?
- [ ] Are all touch targets at least 44x44px?
- [ ] Does text feel comfortable at reading distance (not squinting)?
- [ ] Is horizontal scroll used where stacking would lose context?
- [ ] Is content prioritized (not just rearranged)?
- [ ] Do animations feel smooth (not janky)?
- [ ] Does the mobile nav feel designed (not just a hamburger that opens a list)?
- [ ] Is there anything hidden on mobile that SHOULD be hidden?
- [ ] Is there anything visible on mobile that SHOULDN'T be?
