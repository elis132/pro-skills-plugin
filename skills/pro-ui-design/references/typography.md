# Typography: The 80% Rule

Typography is responsible for roughly 80% of whether a design feels professional or amateur. A mediocre layout with great typography looks designed. A great layout with mediocre typography looks generated.

## How a Senior Designer Thinks About Type

### 1. Font Selection Is Casting

Choosing a font is like casting an actor. You're not picking "a serif" — you're picking a personality that will carry the entire design.

Questions to ask:
- What era does this reference? (A 1960s grotesque feels different from a 2020s geometric sans)
- What cultural context? (A font used by French fashion houses feels different from one used by Silicon Valley startups)
- What physical material would this be printed on? (Letterpress → certain serifs. Swiss poster → certain sans. Screen-first → certain grotesques)
- Does this font have OPINIONS? (Good fonts do. They weren't designed to be invisible — they were designed to say something)

### 2. The Pairing Strategy

Never pair two fonts that are "kind of similar." Pairings need either harmony or contrast:

**Contrast pairings** (most effective for web):
- Display serif + clean geometric sans (e.g., Playfair Display + Space Grotesk)
- Monospace + humanist sans (e.g., JetBrains Mono + Source Sans 3)
- Heavy slab + delicate serif (e.g., Roboto Slab Black + Cormorant Garamond Light)
- Handwritten/display + strict grid sans (e.g., Caveat + IBM Plex Sans)

**Harmony pairings** (for refined, cohesive feels):
- Two weights/styles from the same super-family (e.g., Alegreya + Alegreya Sans)
- Two fonts from the same designer (they often share skeleton proportions)

**What to avoid:**
- Two geometric sans-serifs together (Inter + Roboto = nothing)
- Body font for headlines (just making it bigger isn't typography)
- More than 2 font families (with rare, intentional exceptions)

### 3. Font Recommendations by Direction

**Editoral / Magazine:**
Headlines: Playfair Display, Libre Baskerville, DM Serif Display, Fraunces, Lora
Body: Source Serif 4, Literata, Newsreader, Spectral

**Swiss / Modernist:**
Headlines: Space Grotesk, Outfit, Syne, Epilogue, Satoshi (self-hosted)
Body: IBM Plex Sans, Geist Sans (self-hosted), Work Sans

**Brutalist / Raw:**
Headlines: Space Mono, JetBrains Mono, Darker Grotesque, Anton
Body: Courier Prime, IBM Plex Mono, iA Writer Mono (self-hosted)

**Luxury / Refined:**
Headlines: Cormorant Garamond, Tenor Sans, Cardo, Noto Serif Display
Body: Nunito Sans, Karla, Jost

**Playful / Warm:**
Headlines: Bricolage Grotesque, Syne, Fraunces, Shrikhand
Body: Nunito, Quicksand, Lexend

**Technical / Developer:**
Headlines: JetBrains Mono, Space Grotesk, Geist Mono (self-hosted)
Body: Inter (acceptable here — it was designed for UIs), Geist Sans

**Retro / Vintage:**
Headlines: Righteous, Bungee, Passion One, Abril Fatface
Body: Old Standard TT, Lora, Crimson Pro

**Overused in AI output** (use with extra justification, not banned):
- Inter — great for dense UIs/dashboards. Lazy for everything else.
- Roboto — the Android system font. No personality.
- Open Sans — the "I didn't choose a font" font.
- Poppins — geometric, friendly, and on every third AI-generated site.
- Montserrat — same situation as Poppins.
- Lato — invisible. That's sometimes what you want.

**Overused as "the alternative to overused fonts"** (yes, this is a trap too):
- Space Grotesk — great font, but becoming the AI-alternative-to-Inter default
- DM Serif Display — becoming the AI-alternative-to-sans-serif default
- Cormorant Garamond — becoming the AI "luxury" default

The solution isn't a banned list. It's variety. If you used Space Grotesk in your last design, pick something else this time. Browse Google Fonts sorted by "trending" or "newest" and find something you haven't used. Try Syne, Bricolage Grotesque, Instrument Serif, Cabinet Grotesk (self-hosted), Geist (self-hosted), Epilogue, Outfit, Newsreader, Fraunces, Darker Grotesque. The font catalog is enormous — stop cycling through the same 8 fonts.

### 4. Typographic Scale

Forget Tailwind's linear scale. A professional typographic scale creates DRAMA:

**Wrong (AI default):**
```css
--text-sm: 0.875rem;    /* 14px */
--text-base: 1rem;       /* 16px */
--text-lg: 1.125rem;     /* 18px */
--text-xl: 1.25rem;      /* 20px */
--text-2xl: 1.5rem;      /* 24px */
--text-3xl: 1.875rem;    /* 30px */
```

**Right (designed scale with contrast):**
```css
/* Major third scale with intentional jumps */
--text-caption: 0.75rem;     /* 12px — small, deliberate */
--text-body: 1.0625rem;      /* 17px — slightly larger than default for readability */
--text-lead: 1.25rem;        /* 20px — introductory paragraphs */
--text-subhead: 1.5rem;      /* 24px */
--text-title: 2.5rem;        /* 40px — notice the JUMP */
--text-display: 4.5rem;      /* 72px — dramatic headlines */
--text-hero: 7rem;            /* 112px — statement piece */
```

The key: BIG jumps between levels. A headline at 2.5rem next to body at 1rem creates real hierarchy. A headline at 1.5rem next to body at 1rem creates mush.

### 5. The Details That Matter

**Letter-spacing (tracking):**
- Headlines: often benefit from slight negative tracking (-0.01em to -0.03em) — pulls letters together for cohesion
- ALL-CAPS text: NEEDS positive tracking (+0.05em to +0.15em) — opens up for legibility
- Body text: leave at default (0) unless the font's default is poor
- Small text (captions, labels): slight positive tracking (+0.01em to +0.03em)

**Line-height (leading):**
- Headlines: tight (1.0 to 1.15) — especially large display text
- Body text: generous (1.5 to 1.7) — for readability
- Captions/UI labels: moderate (1.3 to 1.4)
- The default of 1.5 everywhere is wrong. Vary it.

**Font-weight as a tool:**
Don't just use Regular (400) and Bold (700). The full weight range is expressive:
- Thin/Hairline (100): elegant display text, luxury feel
- Light (300): refined, modern body text
- Regular (400): standard body
- Medium (500): subtle emphasis, UI labels
- Semibold (600): subheadings, interactive elements
- Bold (700): strong emphasis
- Black (900): dramatic headlines, brutalist statements

Use at least 3 different weights in a design to create hierarchy.

**Max-width for readability:**
Body text should NEVER stretch across the full viewport. Optimal line length: 50-75 characters.
```css
.prose { max-width: 42rem; } /* ~65 characters at 17px */
```

**Paragraph spacing:**
Don't use `margin-bottom` equal to line-height. Paragraphs need breathing room:
```css
p + p { margin-top: 1.5em; } /* not 1em, not 2em — 1.5em feels natural */
```

### 6. Typography-First Design

The strongest designs often start with typography and let everything else follow:

1. Pick your headline font. Set a massive headline on the page. Does it look good on its own? If not, keep looking.
2. Pick your body font. Write a real paragraph (not lorem ipsum). Does it pair well?
3. Set your scale. Place all sizes on the page together. Is there clear hierarchy?
4. Define your spacing rhythm based on the type metrics.
5. NOW design the layout around the typography.

This inverts the AI approach of "layout first, fill with text later."
