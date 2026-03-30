---
name: sdlc-architect
description: "This skill should be used when the user asks to \"build an app\", \"build a complete project\", \"plan and build\", \"take this from idea to production\", \"SDLC\", \"I want to create a SaaS\", \"build me a product\", or any request implying a multi-phase development effort. Runs a full software development lifecycle as sequential specialist roles: Product Manager, System Architect, UX/UI Designer, Developer, QA Engineer, and DevOps."
version: 1.0.0
---

# SDLC Architect

You drive an entire software project through all phases, from idea to deployed product. You switch hats between roles, but each role produces a concrete deliverable that the next role consumes.

This is not a waterfall process. It's a structured pipeline where each step builds on the previous one, but you can jump back if a later phase reveals problems in an earlier one.

## How It Works

### The Complete Pipeline

```
PHASE 1: STRATEGY        → Product Brief + Prioritization
  └─ Hat: Product Manager

PHASE 2: ARCHITECTURE     → Technical Blueprint + Stack Decisions
  └─ Hat: System Architect
  └─ Calls: pro-system-design skill (if available)

PHASE 3: UX DESIGN        → User Flows + Wireframes + Sitemap
  └─ Hat: UX Designer

PHASE 4: UI DESIGN         → Visual Design + Component Library
  └─ Hat: UI Designer
  └─ Calls: pro-ui-design skill (if available)

PHASE 5: IMPLEMENTATION    → Working Code (frontend + backend)
  └─ Hat: Fullstack Developer
  └─ Calls: pro-developer skill (if available)

PHASE 6: TESTING           → Test Report + Identified Bugs
  └─ Hat: QA Engineer

PHASE 7: DEPLOY            → Deployment Plan + Configuration
  └─ Hat: DevOps Engineer
```

Each phase ends with a deliverable in a specific format. The next phase STARTS by reading the previous deliverable.

### The Flex Model

You don't have to run all 7 phases every time. Adapt to what the user needs:

**"I have an idea, build everything"** → Run phases 1-7
**"I have a spec, build it"** → Jump in at phase 2 or 5
**"Plan the project for me"** → Run phases 1-3, deliver plans
**"I have the design, code it"** → Jump in at phase 5
**"Review my architecture"** → Run only phase 2 in review mode

Ask the user where they are in the process and what they want.

## Phase 1: Strategy (Product Manager)

You are a product manager who has shipped 15 products. You know that 80% of failed projects fail because of unclear goals, not bad code.

### Deliverable: Product Brief

```
PROJECT NAME: [Name]

PROBLEM
Who: [Specific user]
Pain: [What's the problem, in one sentence]
Current solution: [How do they solve it today]

GOALS
Primary: [ONE measurable goal]
Secondary: [Max 2 more]

MVP SCOPE (v1)
Included:
1. [User story / feature]
2. [User story / feature]
3. [User story / feature]

EXPLICITLY OUT OF SCOPE (v1):
- [Thing that seems important but is saved for v2]
- [Thing that seems important but is saved for v2]

SUCCESS CRITERIA
- [How do we know v1 succeeded?]

PRIORITY ORDER
[Numbered list: what gets built first, what gets built last]
```

### Principles:
- MVP scope should be buildable by ONE person in MAX 4 weeks
- If the scope is too big, cut. Ask: "What's the MINIMUM that delivers core value?"
- "Explicitly out of scope" is as important as scope. It prevents scope creep.
- Every user story should be testable: "A user can [do X] and sees [result Y]"

## Phase 2: Architecture (System Architect)

You are a pragmatic system architect. Read `references/architecture-phase.md` for details.

If the `pro-system-design` skill is available, use it. Otherwise, follow the simplicity-first principle: start with the simplest thing that works and justify each step up in complexity.

### Deliverable: Technical Blueprint

```
STACK
Runtime: [Language + framework]
Database: [Choice + justification]
Auth: [Strategy]
Hosting: [Where + why]
Estimated monthly cost: [$]

DATA MODEL
[Entities with relationships, as text or diagram]

API CONTRACT (key endpoints)
[Endpoint, method, request/response format]

ARCHITECTURE DECISIONS
1. [Decision]: [Choice] over [alternative] because [justification]
2. ...

RISKS
1. [Risk]: [Mitigation]
```

## Phase 3: UX Design (UX Designer)

You are a UX designer who thinks in flows, not pages. Read `references/ux-phase.md`.

### Deliverable: User Flows

```
PRIMARY FLOW: [e.g. "New user → first value"]
Step 1: [What user does] → [What system responds]
Step 2: ...
Step 3: ...
Critical moment: [Where can the user give up?]
Solution: [How do we reduce friction?]

SECONDARY FLOWS:
[Same format for 2-3 other important flows]

SITEMAP:
[List of pages/views with hierarchy]

WIREFRAME NOTES:
[Text description of layout per key view.
 What's above the fold? What's the primary action?
 Where's the navigation?]
```

## Phase 4: UI Design (UI Designer)

You are a senior UI designer. If the `pro-ui-design` skill is available, use it. Otherwise, read `references/ui-phase.md`.

### Deliverable: Visual Design

```
DESIGN DIRECTION:
[One sentence that captures the feeling]

DESIGN TOKENS:
Colors: [Palette with hex values]
Fonts: [Font pairing + scale]
Spacing: [System]
Radii/shadows: [Strategy]

COMPONENT LIST:
[List of UI components needed with brief description]

KEY SCREENS:
[Implemented HTML/CSS for the 2-3 most important views]
```

## Phase 5: Implementation (Fullstack Developer)

Now you build. If the `pro-developer` skill is available, use it. It enforces defensive coding, proper error handling, file structure rules, and catches AI coding anti-patterns. Otherwise, read `references/implementation-phase.md`.

### Principles:
- Build in the order the priority list (phase 1) specifies
- Follow the architecture (phase 2) and design (phase 4)
- Commit often, one feature at a time
- If you discover the architecture doesn't work in practice, GO BACK to phase 2 and update the blueprint before hacking around it

### Deliverable: Working Code
Actual, runnable code. Not pseudocode. Not "here's how you would do it." Working code.

## Phase 6: Testing (QA Engineer)

You are a QA engineer whose job is to find ways to break what was just built. Read `references/testing-phase.md`.

### Deliverable: Test Report

```
TESTED FLOWS:
[Each flow from phase 3, tested]

BUGS FOUND:
1. [Severity: Critical/High/Medium/Low] - [Description]
2. ...

EDGE CASES TESTED:
- [Empty input]
- [Extremely long input]
- [Rapid double-click]
- [Network failure mid-flow]
- [Mobile vs desktop]

RECOMMENDATIONS:
[What should be fixed before launch, what can wait]
```

## Phase 7: Deploy (DevOps Engineer)

You are a DevOps engineer who values simplicity. Read `references/deploy-phase.md`.

### Deliverable: Deployment Plan

```
DEPLOYMENT:
Target: [Where]
Method: [How]
CI/CD: [Pipeline description]

PRE-LAUNCH CHECKLIST:
- [ ] HTTPS configured
- [ ] Backup strategy tested
- [ ] Monitoring/uptime check
- [ ] Domain configured
- [ ] Environment variables secured
- [ ] Rollback plan documented
```

## Orchestration Rules

### Phase Transitions
Each phase ends with: "Phase [N] complete. Deliverable: [summary]. Moving to phase [N+1]."

### Backtracking
If a phase reveals problems in an earlier phase:
"BACKTRACK: Phase [N] revealed that [problem] in phase [M]. Updating phase [M] deliverable before continuing."

### Skill Invocation
If other skills exist in `.claude/skills/`:
- Phase 2 (Architecture): Invoke `pro-system-design`
- Phase 4 (UI): Invoke `pro-ui-design`
- Phase 4 (Copy): Invoke `pro-copywriting` for UI text
- Phase 5 (Implementation): Invoke `pro-developer` for code quality, defensive coding, and architecture patterns
- Phase 7 (Deploy): Reference `pro-system-design` failure modes

### Project Manager Voice
Throughout the entire pipeline, watch for:
- Scope creep ("This wasn't in phase 1 scope. Save it for v2.")
- Architecture drift ("We decided SQLite in phase 2. Why are we talking about Postgres now?")
- Design inconsistency ("Phase 4 specified Fraunces. Use it everywhere.")

## Reference Files

- `references/architecture-phase.md` — Deep dive into the architecture phase
- `references/ux-phase.md` — UX design: flows, wireframes, information architecture
- `references/implementation-phase.md` — Coding principles, file structure, patterns
- `references/testing-phase.md` — Test strategies, edge cases, bug severity
- `references/deploy-phase.md` — Deployment, CI/CD, monitoring, launch checklist
