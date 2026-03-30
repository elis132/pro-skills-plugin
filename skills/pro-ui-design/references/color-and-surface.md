# Color & Surface: Beyond the Preset Palette

## How Professionals Think About Color

A senior designer doesn't pick colors from a palette picker and call it done. Color is atmosphere, emotion, and strategy.

### 1. Color Strategy Comes Before Color Picking

Before choosing any hex values, define your strategy:

**Monochromatic + single accent:** 90% of the design uses shades of one base color (or neutrals). ONE additional color marks the single most important action/element. This is the most sophisticated approach and the hardest to mess up.

**Desaturated palette with strategic saturation:** Base colors are muted/earthy/greyed. Saturated colors appear only where attention is needed. Creates a calm, premium feel where important things pop naturally.

**High contrast duotone:** Two colors, used boldly. Black and white is the classic. Navy and cream. Forest green and off-white. Simple, powerful, memorable.

**Full color with discipline:** Multiple colors, but each has a specific job (one for navigation, one for CTAs, one for informational, one for success states). Never decorative — always functional.

### 2. Building a Color Palette

**Start with a single color.** Not from a tool — from a reference. What real-world thing does this design reference? The color of concrete? Aged paper? Deep ocean? Scandinavian forest? That's your starting point.

**Derive your neutrals from your brand color:**
```css
/* DON'T: Generic gray */
--neutral-100: #f5f5f5;
--neutral-900: #1a1a1a;

/* DO: Warm neutrals derived from brand color */
--neutral-100: #f7f5f2; /* hint of warmth */
--neutral-200: #e8e4df;
--neutral-800: #2c2825; /* dark brown-black, not pure black */
--neutral-900: #1a1714;
```

Tinting your neutrals with your brand color makes the palette feel cohesive. Pure grays next to warm colors look dead. Pure grays next to cool colors look harsh.

**Never use pure black (#000000)** for text on white. Use a very dark version of your darkest color (e.g., #1a1714 or #0f172a). The difference is subtle but significant — pure black is too harsh for screens.

**Never use pure white (#ffffff)** for backgrounds if your design is warm. Use a tinted white (#faf8f5, #f9fafb). The eye notices.

### 3. Color Proportions

The 60-30-10 rule exists because it works:
- **60%**: Dominant color (usually your neutral/background)
- **30%**: Secondary color (sections, cards, supporting elements)
- **10%**: Accent (CTAs, highlights, the thing you want people to see)

AI designs often use 5+ colors at roughly equal proportions. This creates visual noise. Be ruthless — fewer colors, used more intentionally.

### 4. Dark Mode Done Right

Dark mode is NOT "invert everything":
- Background should be dark gray (#0a0a0a to #1a1a1a), not pure black
- Text should be off-white (#e5e5e5), not pure white — pure white on dark backgrounds causes eye strain
- Reduce saturation of colors slightly — saturated colors on dark backgrounds vibrate/glow
- Shadows don't work the same — use subtle lighter glows or border instead
- Increase contrast between text and background to at least 7:1 ratio

### 5. Surface and Texture

The background of a design is not just a color — it's a SURFACE.

**Subtle noise/grain:**
```css
.surface-grain {
  background-color: var(--bg);
  background-image: url("data:image/svg+xml,..."); /* inline SVG noise pattern */
  /* Or: use a CSS filter on a pseudo-element */
}
.surface-grain::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
  pointer-events: none;
  z-index: 1;
}
```

**Subtle gradient meshes:**
Instead of flat backgrounds, use very subtle radial gradients that create depth:
```css
.surface-mesh {
  background:
    radial-gradient(ellipse at 20% 50%, rgba(120, 90, 60, 0.08) 0%, transparent 50%),
    radial-gradient(ellipse at 80% 20%, rgba(60, 90, 120, 0.06) 0%, transparent 50%),
    var(--bg-base);
}
```

**Material references:**
- Paper: slightly warm white, subtle grain texture, soft shadows
- Glass: blur backdrop, subtle border, slight transparency
- Concrete: cool gray, noise texture, flat (no shadows)
- Metal: subtle linear gradient, sharp reflections, clean edges
- Fabric: warm tones, very subtle woven pattern, soft everything

### 6. Elevation and Depth

Instead of box-shadow on everything, create depth through:

**Background color layers:**
```css
--surface-0: #f7f5f2; /* page background */
--surface-1: #ffffff; /* cards, raised elements */
--surface-2: #f0ede8; /* inset, recessed elements */
```

**Border as elevation:**
A 1px border in a slightly darker shade creates more elegant elevation than a shadow:
```css
.card {
  border: 1px solid rgba(0, 0, 0, 0.06);
  /* Cleaner than box-shadow for subtle elevation */
}
```

**Shadow only when it matters:**
```css
/* Navigation shadow — only element that needs floating */
.nav {
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
}

/* Modal/dialog — needs heavy shadow for overlay effect */
.modal {
  box-shadow:
    0 25px 50px -12px rgba(0, 0, 0, 0.25),
    0 0 0 1px rgba(0, 0, 0, 0.05);
}

/* Everything else: no shadow. Use border or background instead. */
```

### 7. Color Accessibility

Not optional — this is baseline professionalism:
- Text on background: minimum 4.5:1 contrast ratio (WCAG AA)
- Large text (18px+ bold, 24px+ regular): minimum 3:1
- Interactive elements: minimum 3:1 against adjacent colors
- Never rely on color alone for information (add icons, patterns, text)

Use a contrast checker. If your beautiful desaturated palette fails contrast — adjust until it passes. Accessibility is not negotiable.

### 8. Color Palette Examples by Direction

**Warm editorial:**
```css
--ink: #1a1714;
--paper: #f5f0e8;
--accent: #c45a3c;
--muted: #8a7e72;
--rule: #d4cdc2;
```

**Cool technical:**
```css
--text: #0f172a;
--bg: #f8fafc;
--primary: #0ea5e9;
--muted: #64748b;
--border: #e2e8f0;
```

**Luxury dark:**
```css
--text: #e8e0d4;
--bg: #0c0b09;
--gold: #c4a46c;
--muted: #6b6258;
--surface: #1a1814;
```

**Nature/organic:**
```css
--text: #1a2e1a;
--bg: #f4f1ec;
--green: #3d6b4f;
--earth: #8b7355;
--light: #e8e2d8;
```

**Brutalist high-contrast:**
```css
--text: #000000;
--bg: #ffffff;
--accent: #ff0000;
--highlight: #ffff00;
/* That's it. No subtle shades. Raw. */
```
