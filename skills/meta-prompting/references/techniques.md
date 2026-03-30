# Techniques: Patterns That Reliably Improve Output

## 1. The Good/Bad Example Pair

The single most effective technique. Show one example of what you want and one of what you don't want, with an explanation of WHY.

```
Good: "Your server crashed at 3am. You found out from a tweet."
Bad: "In today's digital landscape, monitoring has never been more important."

The good version starts with a concrete scenario the reader recognizes. 
The bad version starts with vague context that could apply to anything.
```

This works because models are excellent pattern-matchers. An example gives them a concrete target to match. An explanation of why teaches the underlying principle so they can apply it to new situations.

**Tips:**
- Use examples from the actual domain, not generic ones
- The bad example should be a REALISTIC bad example (what the model would actually produce without guidance), not a strawman
- 2-3 pairs is ideal. More than 5 pairs starts to overwhelm

## 2. Identity + Motivation

Don't just say who the model is. Say what they CARE about.

```
Weak: "You are a copywriter."
Better: "You are a senior copywriter."
Best: "You are a senior copywriter who believes the best copy 
is invisible. You care about clarity over cleverness. You'd rather 
cut a brilliant sentence that doesn't serve the reader than keep 
it because it sounds good."
```

Motivation shapes decision-making. When the model encounters an ambiguous choice (flashy sentence vs. clear sentence?), the motivation tips the balance.

## 3. The "What Would X Do?" Frame

Instead of listing rules, describe what a specific type of person would do.

```
Instead of:
"RULE 1: Use specific numbers. RULE 2: Avoid buzzwords. RULE 3: 
Start with the benefit, not the feature. RULE 4: ..."

Write:
"Write like a founder who's explaining their product to a skeptical 
developer friend over coffee. They'd use specific numbers because 
they know their metrics. They wouldn't use buzzwords because their 
friend would roast them. They'd start with the problem because 
that's what their friend cares about."
```

This encodes multiple rules in a single, memorable frame. The model can apply the frame to new situations that your rules didn't explicitly cover.

## 4. Progressive Refinement

Don't try to get perfect output in one shot. Structure the prompt as a pipeline.

```
Step 1: "Write a first draft of the headline. Give me 5 options."
Step 2: "Option 3 is closest. Now refine it: make it shorter and 
more specific. Give me 3 variations."
Step 3: "Take variation 2 and write a subheadline to pair with it."
```

This works because each step gives the model a narrower problem to solve. It also lets you steer mid-process instead of hoping the first output is right.

## 5. Constraint by Demonstration

Instead of saying "don't do X," demonstrate what you DO want. The undesired output becomes unlikely because the desired pattern is so clear.

```
Instead of:
"Don't use em-dashes. Don't start with 'In today's.' Don't use 
buzzwords. Don't write setup sentences before the content."

Write (with example):
"Here's the style I want:

'Your server crashed at 3am. You found out from a tweet. That 
shouldn't happen.

Pulseline checks every 30 seconds from 12 regions. Setup is one 
curl command. Alert hits your phone in 45 seconds.'

Notice: short sentences. Concrete details. No preamble. Starts 
with the problem, not with context."
```

The example implicitly encodes all four constraints without a single "don't."

## 6. Audience-First Framing

Describe who will READ the output. This shapes tone, vocabulary, assumed knowledge, and detail level more effectively than explicit instructions.

```
"The reader is a backend developer evaluating monitoring tools at 
11pm. They've already looked at 5 options today. They're skeptical 
of marketing language. They want to know: does it work, how hard 
is setup, what does it cost. If the first paragraph doesn't answer 
at least one of those, they'll close the tab."
```

This single paragraph replaces: "Be concise. Be technical. Avoid marketing language. Include pricing information. Get to the point quickly."

## 7. The "What Would You Never Say?" Technique

For voice/style prompts, defining what the persona would NEVER say is surprisingly effective.

```
"This person would never say: 'We're passionate about...' or 
'Our innovative solution...' or 'Whether you're a beginner or 
expert...' They would rather close the company than put 'leverage' 
in marketing copy."
```

This creates a negative space that shapes the output. It's especially useful for avoiding AI-default phrases.

## 8. Format Anchoring

Show the exact output format you want with placeholder content. The model will match the format almost exactly.

```
"Format your response exactly like this:

**[Company Name]**
Stack: [1-2 sentence tech stack description]
Why it works: [1 sentence on why this architecture fits]
Watch out for: [1 sentence on the main risk]
Monthly cost: [estimate]"
```

This is more reliable than describing the format in prose. Models match demonstrated formats with very high fidelity.

## 9. Reasoning Before Output

Ask the model to think before it produces the final output. This improves quality for complex tasks.

```
"Before writing the copy, analyze:
1. What is the reader's main objection?
2. What specific detail would overcome that objection?
3. What's the ONE thing they should remember?

Then write the copy based on your analysis."
```

This works because the analysis primes the model with relevant reasoning. The final output is informed by the analysis rather than being a cold start.

## 10. Temperature Control Through Specificity

You can't set the temperature in a prompt, but you can control output variability through specificity.

**More variable output (creative tasks):**
Give identity, motivation, and loose constraints. Don't specify format rigidly. Use phrases like "surprise me" or "find an unexpected angle."

**Less variable output (consistent tasks):**
Give exact format with examples. Specify vocabulary. Constrain length. Use phrases like "follow this pattern exactly."

## 11. Chunking Long Instructions

For skills or system prompts, don't put everything in one block. Organize into labeled sections with clear headers. The model processes structured documents better than walls of text.

```
## Identity
[who you are]

## Task
[what to produce]

## Quality Standards
[what good looks like]

## Common Mistakes
[what to watch for]
```

Each section is a self-contained concept. The model can reference back to the relevant section when generating output.

## 12. The Exit Ramp

For multi-step pipelines, include explicit criteria for when to STOP or when the output is "done."

```
"Review your output against these criteria. If all pass, you're done:
- [ ] Headline is under 10 words
- [ ] Subheadline contains a specific number
- [ ] No buzzwords from the banned list
- [ ] Reads naturally when spoken aloud

If any fail, revise before presenting."
```

Without exit criteria, models either over-iterate (polishing endlessly) or under-iterate (presenting first drafts as final).
