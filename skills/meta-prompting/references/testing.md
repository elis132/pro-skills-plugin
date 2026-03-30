# Testing: How to Iterate on Prompts

## The Testing Mindset

A prompt is a hypothesis: "If I give these instructions, the model will produce this quality of output." Testing is how you validate or invalidate that hypothesis.

Don't test by running once and checking if it's good. Test by running multiple times with varied inputs and looking for PATTERNS in the failures.

## The Basic Test Protocol

### 1. Define "Good"
Before testing, write down what good output looks like. Be specific:
- "The headline is under 10 words and contains a specific benefit"
- "The architecture uses at most 3 technologies"
- "The copy doesn't contain any words from the AI-tells list"

If you can't define "good" before testing, you can't evaluate the results.

### 2. Run 3-5 Times
Same prompt, same model. Check:
- **Consistency:** Are all outputs roughly the same quality? If one is great and one is terrible, the prompt is too ambiguous.
- **Pattern detection:** What do all outputs have in common? Those commonalities are what the prompt reliably produces (good or bad).
- **Failure patterns:** Where does the output fail? Is it the same failure each time?

### 3. Vary the Input
Run with 3 different inputs that span the skill's domain:
- One typical case ("build a SaaS landing page")
- One edge case ("build a landing page for a funeral home")
- One minimal case ("build a page")

Check: Does the skill produce good output for all three? The minimal case tests whether the skill can handle ambiguity. The edge case tests whether it can adapt.

### 4. Check for House Style
Put all outputs side by side. Do they look/feel like the same person made them? Some consistency is fine (that's the skill's value). But if they're TOO similar, the skill is creating a template, not a framework.

Specifically check:
- Same color palette across outputs?
- Same sentence structures?
- Same opening pattern?
- Same level of energy/enthusiasm?
- Same format/layout?

If more than 2 of these are "yes," add more variation guidance.

## Debugging Specific Failures

### "The output ignores my instruction about X"

**Step 1:** Find the instruction in your prompt. How far from the beginning or end is it? Instructions in the middle of long prompts get dropped.

**Step 2:** Is it contradicted by another instruction? "Be concise" + "Include comprehensive details" = one of these will be ignored.

**Step 3:** Is it a negative instruction? ("Don't use X") Negative instructions are weaker than positive ones. Replace with what you DO want.

**Step 4:** Is it too subtle? "Consider the audience" is easy to ignore. "The reader is a 45-year-old CFO who will skim this in 15 seconds" is hard to ignore.

**Fix:** Move the instruction to the top or bottom of the prompt. Make it more specific. Make it positive. Make it part of the identity or quality definition rather than a standalone rule.

### "The output is technically correct but feels off"

**Step 1:** Check the identity. Is the persona specific enough? "A writer" produces different output than "a writer who spent 10 years at The Economist and believes clarity is a moral obligation."

**Step 2:** Check for competing instructions. The model might be trying to satisfy conflicting requirements and producing a compromise that satisfies none of them.

**Step 3:** Add an example. Show one paragraph of exactly the tone/style you want. This is often more effective than any amount of description.

**Fix:** Strengthen the identity. Add a style example. Remove contradictions.

### "Every output looks the same"

**Step 1:** Check if the prompt has a single strong example that the model is anchoring to. One example creates a template. Two contrasting examples create a range.

**Step 2:** Check if constraints are too tight. "MUST use serif. MUST be monochrome. MUST have generous whitespace." = one possible output.

**Step 3:** Check if the skill lacks variation guidance. Does it explicitly encourage different approaches for different contexts?

**Fix:** Add multiple contrasting examples. Loosen constraints. Add a "sameness detector" that compares to previous outputs. Add a "range" section with different possible directions.

### "The first output is good but subsequent ones get worse"

**Step 1:** Context window pollution. Previous outputs (and your feedback) are in context, and the model is being influenced by them.

**Step 2:** The model is learning your preferences from feedback and converging on what it thinks you want. This can be a local optimum that's not globally good.

**Fix:** For skills/system prompts (used across sessions), this isn't an issue. For in-conversation prompts, consider starting fresh for each attempt rather than iterating in the same thread.

## The Iteration Framework

### What to change first
When a prompt isn't working, change ONE thing at a time. This is how you learn what works.

Priority order:
1. **Add an example** (most impactful single change)
2. **Strengthen the identity** (shapes everything downstream)
3. **Clarify the task** (ambiguity is the #1 cause of bad output)
4. **Remove contradictions** (conflicting instructions degrade quality)
5. **Add constraints** (only if output is too variable)
6. **Remove constraints** (only if output is too mechanical)

### When to stop iterating
- Output is consistently good across 3-5 runs
- Output handles edge cases reasonably
- Output doesn't show a house style (or the house style is intentional)
- Improving the prompt further would require a disproportionate amount of effort

### When to start over
- The prompt is over 2000 words and still not working
- You've iterated 5+ times without meaningful improvement
- The core approach feels wrong (adding patches to a flawed foundation)

Start over with: a clear identity, one strong example, and the minimum constraints needed. Build up from there.

## Documentation

Keep a log of what you tried and what happened:

```
Version 1: Basic prompt. Output was generic.
Version 2: Added identity. Tone improved, still too vague.
Version 3: Added good/bad example pair. Quality jumped significantly.
Version 4: Added anti-pattern list. Model became too cautious.
Version 5: Replaced anti-pattern list with "intention over avoidance."
           Quality is now consistently good.
```

This log prevents you from going in circles and helps you understand which techniques work for which types of prompts.
