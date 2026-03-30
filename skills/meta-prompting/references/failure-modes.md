# Failure Modes: Why Prompts Fail

## Diagnosing the Problem

When AI output isn't good, the problem is almost always the prompt, not the model. Here's how to diagnose and fix each type of failure.

## Failure 1: Generic Output

**Symptom:** The output is correct but bland. It could apply to any project/company/context. It reads like a template.

**Root cause:** The prompt doesn't define what "good" looks like for THIS specific case. The model produces the statistical average of all similar outputs in its training data.

**Fixes:**
- Add specific context (company name, product details, audience specifics)
- Add a good/bad example pair showing the quality you want
- Add identity with motivation ("you care about X specifically because Y")
- Replace vague instructions ("be creative") with specific ones ("include at least one metaphor comparing the product to a physical object")

**Test:** Remove the brand/company name from the output. Could it work for a competitor? If yes, it's too generic.

## Failure 2: Rule-Following Without Understanding

**Symptom:** The output technically follows every rule but feels mechanical, lifeless, or forced. Like a student who follows the rubric perfectly but misses the point.

**Root cause:** Too many explicit rules. The model optimizes for compliance instead of quality. Each MUST/NEVER/ALWAYS adds cognitive load that crowds out creativity.

**Fixes:**
- Reduce rules by 50%. Keep only the ones that truly matter.
- Replace rules with examples. One example teaches the spirit. Rules teach the letter.
- Replace "MUST do X" with "X matters because [reason]." Explanation creates understanding. Commands create compliance.
- Add more identity/motivation, less constraint.

**Test:** Read the output ignoring the rules. Does it feel natural? If not, the rules are the problem.

## Failure 3: Instruction Dropout

**Symptom:** The model follows some instructions but ignores others. Especially common in long prompts.

**Root cause:** Prompt is too long, instructions are buried in the middle, or instructions contradict each other.

**Fixes:**
- Put the most important instructions at the TOP and BOTTOM of the prompt (attention curve)
- Remove contradictory instructions. "Be concise" and "Be comprehensive" can't coexist. Pick one.
- Reduce total instruction count. If you have 20 rules, the model WILL drop some. Prioritize the 5 that matter most.
- Structure with headers and sections. Models process structured text better than prose.
- Move detailed instructions to the point where they're needed (progressive disclosure), not upfront.

**Test:** Check each instruction one by one. Is the output following it? Highlight the ones being dropped. Are they buried in the middle? Are they contradicted by something else?

## Failure 4: Wrong Tone/Style

**Symptom:** The content is right but the voice is wrong. Too formal, too casual, too enthusiastic, too robotic.

**Root cause:** The prompt specifies WHAT to produce but not WHO is producing it or FOR WHOM.

**Fixes:**
- Add identity. Not just "be a copywriter" but a specific copywriter with specific opinions.
- Add audience. Not just "write for developers" but "write for a tired developer at 11pm who's skeptical of marketing."
- Add a style example. Even one paragraph of "this is the tone I want" reshapes everything.
- Specify what this voice would NEVER say. Negative constraints on voice are very effective.

**Test:** Read the output in the voice of the intended persona. Does it sound like something they'd actually say?

## Failure 5: Inconsistency Between Runs

**Symptom:** Running the same prompt multiple times produces wildly different quality. Sometimes great, sometimes terrible.

**Root cause:** The prompt is ambiguous. The model has multiple valid interpretations and randomly picks one each time.

**Fixes:**
- Add format anchoring (show the exact output structure you want)
- Add a concrete example of good output
- Reduce ambiguous terms ("creative," "interesting," "good") and replace with specific criteria
- Constrain length and structure more tightly

**Test:** Run the prompt 5 times. If quality variance is high, the prompt is too ambiguous. Add specificity until runs converge.

## Failure 6: Refusal or Over-Caution

**Symptom:** The model hedges, adds unnecessary disclaimers, refuses to take a position, or says "I can't do that" for things it clearly can do.

**Root cause:** The prompt triggers safety-related patterns without actually being unsafe. Common triggers: anything medical, legal, financial, or controversial. Also triggered by aggressive "you MUST" language that makes the model defensive.

**Fixes:**
- Frame the task positively. "Write a persuasive argument for X" instead of "argue for X" (which can trigger debate-avoidance).
- Provide context that makes the task clearly legitimate. "For a blog post about..." or "For a product description..."
- Reduce aggressive language (ALL CAPS, excessive MUSTs, threatening framing)
- Ask for the output "in the voice of [expert]" rather than asking the model to BE the expert making real decisions.

## Failure 7: Hallucination / Making Things Up

**Symptom:** The model includes fake facts, nonexistent sources, or made-up details.

**Root cause:** The prompt asks for specific factual information without providing it, OR the prompt's identity framing makes the model feel it should know things it doesn't.

**Fixes:**
- Provide the facts you want included. Don't make the model guess your company's metrics.
- Add "Only include facts I've provided. If you need to reference something I haven't given you, use a placeholder like [INSERT METRIC]."
- For creative tasks: separate fact from fiction explicitly. "The company details are real. The testimonial quotes can be fictional but realistic."

## Failure 8: Same Output Every Time (House Style)

**Symptom:** No matter what you ask for, the output has the same feel. Same sentence structures, same vocabulary, same level of enthusiasm.

**Root cause:** The identity or style instructions are too narrow, or the model has settled into a comfortable pattern.

**Fixes:**
- Change the identity for different tasks. A "grumpy engineer" produces different output than a "enthusiastic product manager."
- Explicitly request variation: "This should feel completely different from [previous output]."
- Provide a contrasting example from a different domain.
- Challenge the model: "What would the OPPOSITE approach look like?"

## The Meta-Pattern

Most prompt failures share one root cause: the prompt tells the model WHAT to produce without giving enough context about WHY, FOR WHOM, and WHAT GOOD LOOKS LIKE.

Adding those three elements fixes 80% of prompt problems.
