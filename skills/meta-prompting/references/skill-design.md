# Skill Design: Building Instruction Sets That Work

## What Makes a Good Skill

A skill is a reusable instruction set that consistently produces expert-level output in a specific domain. Based on building and testing multiple skills, here are the patterns that work.

## Architecture

### The Two-Layer Structure

**Layer 1: SKILL.md (the orchestrator)**
- Identity and philosophy (who is the model in this domain)
- The pipeline (what steps to follow, in what order)
- When to read which reference file
- Final checklist before presenting output
- Under 500 lines. Ideally under 300.

**Layer 2: Reference files (the knowledge)**
- Detailed guidance for specific aspects
- Only loaded when relevant (progressive disclosure)
- Can be longer. 200-400 lines each is fine.
- Each reference file should be self-contained and focused on one topic.

This two-layer structure works because the model doesn't need all information at once. It reads SKILL.md, starts the pipeline, and loads reference files as needed. This keeps context focused and reduces instruction dropout.

### The Pipeline Pattern

Every effective skill we've built follows the same meta-pattern:

```
1. THINK (understand the specific context before acting)
2. DECIDE (make design/strategy decisions with rationale)
3. EXECUTE (produce the actual output)
4. CRITIQUE (review against quality standards)
5. ITERATE (fix problems before presenting)
```

The specific names change per domain (Design Brief / Direction / Implementation / Self-Critique), but the structure is the same. The key insight: steps 1-2 PREVENT the model from defaulting to average output. Without them, the model jumps straight to step 3 and produces generic results.

### Reference File Design

Each reference file should:
- Focus on ONE topic (typography, failure modes, API design, etc.)
- Start with the WHY (why this topic matters)
- Include concrete examples (good/bad pairs)
- Be written as guidance, not as rules
- Include a self-check or checklist at the end
- Be readable standalone (someone should understand it without reading SKILL.md first)

## Writing Principles

### Explain Why, Not Just What

```
Bad:  "NEVER use em-dashes."
Good: "Em-dashes have become one of the most recognizable AI tells.
      Real humans rarely use them in casual or commercial text.
      Maximum one per page. Zero is better."
```

The "good" version explains the reasoning. This helps the model apply the principle to new situations the rule doesn't explicitly cover. If the model understands that em-dashes are bad because they signal AI, it will also avoid other AI-signal patterns even if they're not listed.

### Use "Intention Over Avoidance"

```
Bad:  "Don't use gradients. Don't use rounded corners. Don't use 
      shadows. Don't use Inter. Don't use purple."

Good: "Every visual choice must be intentional. A gradient is fine 
      if you chose it for a specific atmospheric effect. It's bad 
      if it's there because 'that's what websites look like.' The 
      test: can you explain why this specific choice is right for 
      THIS specific project?"
```

Long "don't" lists create a paranoid model that avoids everything on the list but doesn't understand the underlying principle. The intention-based approach teaches the model to THINK about choices instead of pattern-matching against a blocklist.

### Include the Anti-Pattern of Your Own Skill

This is the hardest but most important lesson. Every skill creates a "house style" that becomes its own cliché if unchecked. Explicitly acknowledge this.

The pro-ui-design skill learned this the hard way: it avoided AI-generic design by always producing muted-serif-editorial designs. The fix was adding "Failure Mode 2: Anti-AI Overcorrection" that explicitly warned against the skill's own default.

For every skill you build, ask: "If someone ran this skill 10 times in a row, what patterns would emerge?" Then add a section that warns against those specific patterns.

### The Specificity Gradient

Instructions should be specific where output quality is most sensitive, and loose where the model's creativity adds value.

```
High specificity (sensitive areas):
- Format requirements ("Maximum one em-dash per page")
- Quality standards ("Every claim must have a specific detail")
- Failure patterns to avoid ("The Three-Card Grid is never acceptable")

Low specificity (creative areas):
- Visual direction ("Express the direction as a sentence that captures the FEELING")
- Voice ("Write like a founder explaining to a skeptical friend")
- Problem framing ("What's the ONE thing someone will remember?")
```

## Common Skill Design Mistakes

### Mistake 1: Too Many Rules
Skills with 30+ explicit rules produce rigid, mechanical output. The model spends its "attention budget" on compliance instead of quality. Cut to the 10 rules that matter most. Convert the rest to examples or philosophy.

### Mistake 2: No Examples
Rules without examples are ambiguous. "Use good typography" means nothing. "Here's good typography: Playfair Display at 72px with -0.025em tracking for headlines, Literata at 17px for body" is actionable.

### Mistake 3: No Self-Critique Step
Without a critique step, the model presents first drafts as final work. The critique step forces a review pass that catches the most common failures. It also enables iteration within a single run.

### Mistake 4: No Anti-Pattern for the Skill Itself
Every skill develops a default. If you don't explicitly call it out, every output will converge on the same patterns. Include a "sameness detector" that compares current output to previous outputs.

### Mistake 5: Ignoring the Attention Curve
The model pays most attention to the beginning and end of any text. Skills that put critical instructions in the middle of a long document will see those instructions ignored. Put the most important guidance in SKILL.md (read first) and in the final checklist (read last). Put details in reference files (read when relevant, in focused context).

### Mistake 6: Static Quality Definition
"Good" changes based on context. A skill that defines "good" as one thing (e.g., "refined and minimal") produces homogeneous output. Better: define "good" as context-dependent ("the design should match the energy of what it's for").

## Skill Testing Protocol

1. **Run 3 different prompts** that represent the skill's domain
2. **Check for house style:** Do all three outputs share patterns that aren't dictated by the brief?
3. **Check the worst output:** The skill's quality floor matters more than its ceiling
4. **Check edge cases:** What happens with unusual or minimal prompts?
5. **Compare to without-skill:** Is the output actually better, or just different?
6. **Iterate:** Fix the specific failures you find, then test again
