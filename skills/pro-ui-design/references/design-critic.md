# Design Critic: Self-Review Rubric

## The Three-Second Test

Look at the design for exactly three seconds, then look away.
1. What was the FIRST thing you noticed? (If nothing stands out, the design lacks a focal point)
2. What MOOD did it give you? (If the answer is "professional" or "clean" — those aren't moods. Those are absences of mood.)
3. Could you describe it to someone in a way that distinguishes it from other websites? ("It's the one with [X]")

## Critical Failure Modes

Check for ALL THREE of these. Any one is a fail:

### Failure Mode 1: AI Generic
The Tailwind/SaaS default. Signs:
- Could swap the logo and it works for any company
- Purple/blue gradient anywhere
- Three-card feature grid with Lucide icons
- "Start free trial" hero with product mockup
- Inter, Roboto, or Poppins

### Failure Mode 2: Anti-AI Overcorrection
The "tasteful" template that avoids everything AI does — but creates its own template. Signs:
- Warm monochrome palette with one desaturated accent
- Serif display font + light-weight sans body
- Ruled line dividers between every section
- Single-column editorial scroll
- "Restrained" everything — minimal color, minimal animation, minimal personality
- Feels like it was designed for an architecture firm regardless of what it's actually for

**This failure mode is sneaky because it FEELS like good design.** Ask: would this same visual approach work just as well for 10 completely different businesses? If yes, it's a template — a sophisticated one, but still a template.

### Failure Mode 3: Random Boldness
Overcorrecting for overcorrection — being "bold" without purpose. Signs:
- Neon colors that don't relate to the brand/context
- Animations on everything for no reason
- Layout chaos that confuses rather than delights
- "Experimental" for the sake of experiment
- Feels like a design student's portfolio piece, not a real solution

The goal is INTENTIONAL boldness — where every risky choice is in service of the design concept.

## Category Review

### Concept
- Can you state the design concept in one specific sentence?
- Does that sentence contain something CONCRETE and UNIQUE? (Not "clean and modern" — that means nothing)
- Does every major design decision trace back to that concept?
- Would someone who read only the concept recognize this as the output?

### Energy Match
This is the most important category and the one most often failed:
- Does the visual energy match the SUBJECT? A kids' game site should feel playful. A punk label should feel aggressive. A spa should feel calming. A fintech startup should feel ambitious.
- Is the energy SPECIFIC? "Professional" is not an energy. "Quietly confident" is. "Aggressively competent" is. "Warmly chaotic" is.
- Would the target audience feel at home here? A 22-year-old creative would bounce from a site that feels like it was designed for a 50-year-old lawyer, and vice versa.

### Typography
- Is the font choice specific to THIS project (not your default go-to)?
- Does the typographic scale create real hierarchy (big jumps, not linear increments)?
- Are letter-spacing, line-height, and weight varied with intention?
- Would changing the font to something generic noticeably hurt the design? (If not, the font isn't doing enough work)

### Color
- Does the color strategy match the energy? (Muted ≠ always right. Saturated ≠ always wrong.)
- Can you explain WHY each color exists?
- Is there appropriate contrast and accessibility?
- Would you describe this palette as "unique to this project" or "generally tasteful"? The latter is a warning sign.

### Layout
- Does the layout have at least one non-obvious choice?
- Is the page structure different from a generic template?
- Does the spacing create rhythm (varied density between sections)?
- Is the responsive version DESIGNED, not just stacked?

### Motion
- Does the animation approach match the design's energy?
- Is there VARIETY in motion (not everything using the same reveal)?
- Did you consider NO animation? (Sometimes static is better)
- Did you consider MORE animation? (Sometimes elaborate is better)
- Are you using the same IntersectionObserver scroll-reveal as every other page? If so, either make it different or remove it.

### Memorable Detail
- Is there at least ONE thing someone would remember after closing the tab?
- Is it genuinely surprising, or just "well-crafted"? (Well-crafted is baseline, not memorable)
- Could you share a screenshot of this one element and it would be interesting even without context?

## The Sameness Detector

This is the most important section. Answer honestly:

**Compare to your recent work.** If this is not the first design in the conversation, compare:

| Dimension | This design | Previous design(s) | Same? |
|---|---|---|---|
| Color temperature | ? | ? | ? |
| Light or dark | ? | ? | ? |
| Serif or sans headline | ? | ? | ? |
| Dense or spacious | ? | ? | ? |
| Loud or quiet | ? | ? | ? |
| Animation approach | ? | ? | ? |
| Layout structure | ? | ? | ? |
| Section transitions | ? | ? | ? |

If more than 3 dimensions are "Same" — you've fallen into a pattern. Fix it.

## When to Ship vs. When to Iterate

**Ship if:**
- The concept is clear and executed
- The energy matches the subject
- There's at least one memorable element
- It's genuinely different from your last design
- A senior designer would find it interesting (not just "clean")

**Iterate if:**
- It feels "safe" or "nice" but not distinctive
- The energy is wrong for the audience/subject
- It shares too many traits with your recent work
- You can't identify the memorable element
- It could be a template for any company in the same industry
