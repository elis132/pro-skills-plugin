# Anatomy: The Components of Effective Prompts

## The Five Components

### 1. Identity

Tell the model WHO it is. This shapes everything else.

**Weak identity:**
"You are a helpful assistant."

**Strong identity:**
"You are a senior backend engineer with 15 years of production experience. You've been on-call, debugged systems at 3am, and learned the hard way that simplicity beats cleverness. You have strong opinions about architecture, and you'd rather use boring technology that works than exciting technology that breaks."

Why identity works: the model generates text that's consistent with the described persona. A "senior engineer" produces different output than a "helpful assistant," even for the same question. The persona activates different knowledge and different communication styles.

**Identity tips:**
- Specific beats general. "Senior copywriter who's worked at Ogilvy" beats "good writer."
- Include what they care about. "You care deeply about readability and hate unnecessary jargon" shapes output more than role alone.
- Include what they'd NEVER do. "You would never use buzzwords like 'leverage' or 'synergy'" prevents specific failure modes.
- Match the expertise level to the task. Don't use "world's best" for everything. A "competent, practical engineer" produces different (sometimes better) output than "the most brilliant engineer alive."

### 2. Context

What situation are we in? What exists already? What's the background?

**Weak context:**
"Write a landing page."

**Strong context:**
"We're building a monitoring tool for indie developers. We have 40 paying customers. The site is currently a placeholder. Our competitors are Datadog (expensive, complex) and UptimeRobot (simple but limited). Our differentiator is one-line setup with zero config."

Context tells the model what to assume, what constraints exist, and what decisions have already been made. Without context, the model fills in assumptions. Those assumptions are usually "average."

**Context tips:**
- State what already exists. Don't make the model guess.
- State what the constraints are. Budget, team size, timeline, technology stack.
- State who the audience is. Not "users" but specific humans.
- State what has been tried and didn't work (if relevant). This prevents the model from suggesting the thing you already rejected.

### 3. Task

What specifically should the model produce?

**Weak task:**
"Help me with my website."

**Strong task:**
"Write the hero section copy for our landing page. I need: a headline (under 10 words), a subheadline (one sentence that expands on the headline with a specific detail), and a CTA button label."

**Task tips:**
- Specify the deliverable format. "Write copy" is vague. "Write a headline, subheadline, and CTA label" is specific.
- Specify scope. Is this the whole page or just one section?
- Specify what decisions to make vs. what decisions are already made. "The color scheme is already decided (dark blue). I need help with the layout."

### 4. Constraints

Boundaries, format requirements, things to avoid.

**Constraints that work:**
- Format constraints: "Respond in JSON" / "Use bullet points" / "Maximum 50 words"
- Style constraints: "Write in the voice described above" / "Match the tone of this example"
- Content constraints: "Don't mention competitors by name" / "Focus on the technical audience"

**Constraints that backfire:**
- Too many "NEVER" rules. The model spends attention on what NOT to do instead of what TO do. 5+ "NEVER" rules create a paranoid, hedging model.
- Contradictory constraints. "Be concise" + "Include all details" + "Be comprehensive" forces the model to pick one and ignore the others.
- Unmeasurable constraints. "Be creative" is not a constraint. "Include at least one metaphor" is.

**The "Don't Think of an Elephant" Problem:**
Telling the model "NEVER mention X" sometimes makes it more likely to mention X, because the concept is now activated in context. Better approach: instead of listing what to avoid, describe what you DO want in enough detail that the undesired output simply isn't the most likely continuation.

### 5. Quality Definition

What does "good" look like? This is the most important and most often missing component.

**Weak quality definition:**
"Make it good."

**Strong quality definition (via example):**
"Here's an example of the quality I'm looking for:

Hero headline: 'Know it's down before they do'
Subheadline: '30-second checks from 12 regions. Setup is one curl command.'

Notice: specific numbers, no buzzwords, speaks directly to the reader's problem."

**Strong quality definition (via contrast):**
"Good: 'Your server crashed at 3am. You found out from a tweet.'
Bad: 'In today's digital landscape, monitoring your infrastructure has never been more critical.'

The good version starts with a specific scenario. The bad version starts with vague context."

**Quality definition tips:**
- Examples beat rules. Always. One good/bad pair teaches more than a page of instructions.
- Show the FORMAT you want. If you want JSON, show JSON. If you want a specific heading structure, show one.
- Explain what makes the good example good. Don't just show it. Say WHY it works.

## Component Priority

Not every prompt needs all five components. Here's when each matters most:

| Task Type | Identity | Context | Task | Constraints | Quality Def |
|---|---|---|---|---|---|
| Simple question | Skip | Skip | Minimal | Skip | Skip |
| Creative writing | Critical | Important | Important | Light | Critical |
| Data extraction | Skip | Important | Critical | Critical | Important (examples) |
| Analysis | Important | Critical | Important | Light | Important |
| Code generation | Light | Critical | Critical | Important | Important (examples) |
| Skill/instruction writing | Critical | Important | Critical | Important | Critical |

## Prompt Length

**Short prompts** (1-3 sentences) work for: simple questions, code completions, translations, formatting.

**Medium prompts** (1-2 paragraphs) work for: creative tasks with clear scope, analysis with specific focus, most everyday tasks.

**Long prompts / system prompts** (500+ words) work for: skills, persistent behaviors, complex multi-step tasks, when consistency across many outputs matters.

**The attention curve:** Models pay most attention to the beginning and end of prompts. Long middle sections get less attention. Put the most important instructions at the top and bottom. Put details and examples in the middle.
