# Layout Systems: Beyond the Default Grid

## The Problem

AI defaults to symmetrical grids, alternating left-right sections, and evenly distributed columns. These are the most predictable layouts on the web. A senior designer thinks about layout as COMPOSITION — arranging elements to guide the eye, create tension, and establish hierarchy.

## Layout Principles

### 1. Hierarchy Through Scale Contrast

The most important element should be dramatically larger than everything else. Not 20% larger — 300% larger. This is the single fastest way to make a layout feel designed.

```css
/* AI default: everything is similar size */
.feature-grid { display: grid; grid-template-columns: repeat(3, 1fr); }

/* Designed: one thing dominates */
.feature-layout {
  display: grid;
  grid-template-columns: 2fr 1fr;
  grid-template-rows: auto auto;
}
.feature-hero { grid-row: 1 / -1; } /* Hero spans full height */
.feature-secondary { /* Stacked on the right, clearly subordinate */ }
```

### 2. Asymmetric Grids

Symmetry is safe. Asymmetry is designed. Use unequal column widths:

```css
/* Editorial asymmetry */
.editorial { grid-template-columns: 1fr 2fr; gap: 2rem; }

/* Swiss offset */
.swiss { grid-template-columns: 200px 1fr 1fr; }

/* Sidebar emphasis */
.sidebar-heavy { grid-template-columns: 1fr 3fr; }

/* Golden ratio approximation */
.golden { grid-template-columns: 1fr 1.618fr; }
```

### 3. The Rhythm of Sections

Don't make every section the same height/density. Create rhythm:

```
DENSE section (lots of info, tight spacing)
↓
BREATHING section (one big statement, massive whitespace)
↓
MEDIUM section (moderate density)
↓
FULL-BLEED visual (edge-to-edge image or color)
↓
NARROW text section (max-width: 40rem, centered)
```

Varying section density is what makes a page feel designed vs. templated.

### 4. Breaking the Grid

The most memorable layouts have elements that intentionally break the grid:

**Overflow elements:**
```css
.breakout-image {
  width: calc(100% + 4rem);
  margin-left: -2rem;
  /* Image breaks out of its container */
}
```

**Overlapping elements:**
```css
.overlap-card {
  margin-top: -4rem;
  position: relative;
  z-index: 2;
  /* Card overlaps the previous section */
}
```

**Staggered layouts:**
```css
.staggered-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
.staggered-grid > :nth-child(2) {
  transform: translateY(3rem);
  /* Second item drops down, creating visual interest */
}
```

### 5. Negative Space as Design Element

Whitespace isn't empty — it's structural:

**Extreme whitespace for importance:**
```css
.statement-section {
  padding: 12rem 0;
  text-align: center;
}
.statement-section h2 {
  font-size: 3rem;
  max-width: 20ch; /* Very short line length = dramatic */
  margin: 0 auto;
}
```

**Tight spacing for energy:**
```css
.dense-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 2px; /* Almost no gap — creates visual density and energy */
}
```

### 6. Layout Patterns Worth Knowing

**The Editorial Spread:**
Like a magazine — large hero image, text wraps around smaller images, pull quotes break the text. Uses CSS Shapes or simple floats for text wrapping.

**The Bento Grid:**
Popular in modern design (Apple uses it). NOT just equal boxes — the key is varied sizes: one big tile (2x2), a few medium (2x1 or 1x2), and small (1x1). Each tile has different content density.

**The Scroll Narrative:**
Single-column, scroll-driven storytelling. Full-viewport sections with `scroll-snap-align`. Each section is a "scene." Works brilliantly for product stories, case studies, about pages.

**The Dashboard Layout:**
Dense information display. Use a strict grid with a clear system:
- Narrow fixed sidebar (64px collapsed, 240px expanded)
- Content area with its own grid
- Cards of varying sizes but always snapping to the grid

**The Split Screen:**
Two halves, each with its own purpose. One side sticky, other side scrolls. Or both scroll independently. Creates strong visual drama.

```css
.split {
  display: grid;
  grid-template-columns: 1fr 1fr;
  min-height: 100vh;
}
.split-left {
  position: sticky;
  top: 0;
  height: 100vh;
}
```

**The Single Column (done well):**
Not lazy — intentional. Narrow max-width (35-45rem), beautiful typography, generous line spacing, strategic use of full-bleed elements that break out of the column.

### 7. Responsive Layout Philosophy

Don't just stack columns on mobile. Redesign:

- What's MOST important on a small screen? Lead with that.
- Some elements should disappear on mobile, not just shrink.
- Mobile spacing should feel generous, not cramped-and-stacked.
- Consider horizontal scroll for elements that lose meaning when stacked (comparison tables, timelines).
- Touch targets: 44px minimum. Not 32px. Not "close enough."

### 8. Section Transitions

How sections connect matters:

**Hard cut:** Clean line between sections. Works for high-contrast transitions (dark → light).

**Overlap:** Next section overlaps previous with negative margin or absolute positioning. Creates depth.

**Angle/Clip:** `clip-path` to create diagonal or curved section boundaries. Use sparingly — it's easy to overdo.

**Fade through color:** Gradient background that transitions between section colors. Subtle but effective.

**Content bridge:** An element (image, quote, graphic) that sits between two sections, belonging to both. Creates visual continuity.

## The Layout Decision Framework

Given your design brief, pick your primary layout strategy:

| Design Direction | Primary Layout | Why |
|---|---|---|
| Editorial/magazine | Asymmetric grid + pull quotes | Mimics print editorial |
| Luxury/minimal | Single column with full-bleed breaks | Restraint = elegance |
| Technical/dashboard | Dense grid with clear system | Information density done right |
| Creative/portfolio | Bento or masonry with varied sizes | Showcases visual work |
| Corporate/trust | Structured sections with clear hierarchy | Predictability = trust |
| Startup/energy | Scroll narrative + bold sections | Momentum and story |
| Brutalist/statement | Broken grid + overlapping elements | Rule-breaking is the point |
