# Motion & Interaction: Making It Feel Alive

## Philosophy

Motion in a professional design is never decorative. Every animation either:
1. **Communicates** — shows relationship, hierarchy, or state change
2. **Guides** — directs attention to what matters right now
3. **Delights** — creates a moment of pleasure that reinforces the brand
4. **Orients** — helps the user understand where they are in the interface

If an animation doesn't serve one of these purposes, remove it.

## The AI Animation Problem

AI-generated designs tend to add animation everywhere at the same intensity. Every element fades in, every button scales on hover, every card lifts with a shadow. This creates an anxious, busy interface where nothing feels important because everything is moving.

Professional motion design uses **contrast**: mostly still, with strategic moments of movement.

## Timing and Easing

### Duration Rules
- **Micro-interactions** (hover, toggle, button press): 100-200ms
- **Content transitions** (panel open, tab switch): 200-350ms
- **Page transitions** (route change, modal open): 300-500ms
- **Entrances** (first load, scroll reveal): 400-800ms
- **Complex choreography** (staggered reveals): 600-1200ms total

If you're above 1 second for any single animation, it's probably too slow.

### Easing Functions
Stop using `ease` and `ease-in-out` for everything. Each easing function has a personality:

```css
/* Elements entering the screen — start fast, end gentle */
--ease-out: cubic-bezier(0.16, 1, 0.3, 1);

/* Elements leaving — start gentle, exit quickly */
--ease-in: cubic-bezier(0.55, 0, 1, 0.45);

/* Elements moving within the screen — smooth both ends */
--ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);

/* Snappy interaction response — quick with slight overshoot */
--ease-snap: cubic-bezier(0.2, 0, 0, 1);

/* Bouncy/playful — for casual/fun designs */
--ease-bounce: cubic-bezier(0.34, 1.56, 0.64, 1);

/* Linear — only for looping animations or progress bars */
--ease-linear: linear;
```

**Rule of thumb:** Entrances use ease-out. Exits use ease-in. Movements use ease-in-out. Interactions use snap.

## Patterns That Work

### 1. Staggered Entrance
The single most effective page-load animation. Elements enter sequentially with a consistent delay:

```css
.stagger-enter > * {
  opacity: 0;
  transform: translateY(1.5rem);
  animation: enter 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

.stagger-enter > *:nth-child(1) { animation-delay: 0ms; }
.stagger-enter > *:nth-child(2) { animation-delay: 80ms; }
.stagger-enter > *:nth-child(3) { animation-delay: 160ms; }
.stagger-enter > *:nth-child(4) { animation-delay: 240ms; }

@keyframes enter {
  to { opacity: 1; transform: translateY(0); }
}
```

**Key:** The delay between items (80ms) creates rhythm. Too fast (20ms) and you can't perceive the stagger. Too slow (200ms) and it feels sluggish.

### 2. Scroll-Triggered Reveals
Elements appear as they enter the viewport. Use Intersection Observer, not scroll events:

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target); // Only animate once
    }
  });
}, { threshold: 0.15 }); // Trigger when 15% visible

document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
```

```css
.reveal {
  opacity: 0;
  transform: translateY(2rem);
  transition: opacity 0.7s var(--ease-out), transform 0.7s var(--ease-out);
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
```

**Important:** Don't animate everything on scroll. Pick the 3-4 most important elements per page section.

### 3. Hover States That Mean Something

**For links/text:**
```css
/* Underline draw — elegant, editorial */
a {
  text-decoration: none;
  background-image: linear-gradient(currentColor, currentColor);
  background-size: 0% 1px;
  background-position: left bottom;
  background-repeat: no-repeat;
  transition: background-size 0.3s var(--ease-out);
}
a:hover { background-size: 100% 1px; }
```

**For cards:**
```css
/* Subtle lift with border change — not shadow! */
.card {
  border: 1px solid var(--border);
  transition: border-color 0.2s, transform 0.2s var(--ease-snap);
}
.card:hover {
  border-color: var(--accent);
  transform: translateY(-2px); /* Subtle. Not -8px. */
}
```

**For images:**
```css
/* Slow zoom — cinematic feel */
.image-wrapper { overflow: hidden; }
.image-wrapper img {
  transition: transform 0.8s var(--ease-out);
}
.image-wrapper:hover img {
  transform: scale(1.04); /* Subtle. Not 1.15. */
}
```

### 4. Button Interactions

```css
/* Depth press — feels physical */
.btn {
  transition: transform 0.1s, box-shadow 0.1s;
}
.btn:hover {
  transform: translateY(-1px);
}
.btn:active {
  transform: translateY(1px);
  /* Button "presses in" — physical feedback */
}
```

### 5. Text Reveal Effects

```css
/* Clip reveal — text slides up into view */
.text-reveal {
  overflow: hidden;
}
.text-reveal span {
  display: inline-block;
  transform: translateY(110%);
  transition: transform 0.7s var(--ease-out);
}
.text-reveal.visible span {
  transform: translateY(0);
}
```

## Interaction Design Principles

### Feedback is Non-Negotiable
Every interaction must have immediate visual feedback:
- Click → instant state change (not waiting for API response to show change)
- Hover → within 100ms
- Form error → on the field, not at the top of the page
- Loading → show immediately, not after 500ms of nothing

### Reduced Motion
Always respect the user's preference:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### Touch Targets
Mobile interactive elements: minimum 44x44px. This is not a suggestion. Small touch targets are a hallmark of designs that weren't tested on real devices.

### Cursor as Communication
```css
[role="button"] { cursor: pointer; }
[disabled] { cursor: not-allowed; }
[data-draggable] { cursor: grab; }
[data-draggable]:active { cursor: grabbing; }
.text-selectable { cursor: text; }
.zoomable { cursor: zoom-in; }
```

## What NOT to Animate

- Body text appearing (let it be there instantly)
- Navigation elements (they should be immediately available)
- Critical information (error messages, alerts)
- Things that animate EVERY time (repeated hover animations on the same session become annoying)
- Background colors of entire sections (subtle is fine, dramatic is jarring)
- Font sizes or font weights (causes layout thrashing and looks broken)
