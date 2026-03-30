# UX Phase: Flows Before Pixels

## Principle

UX is not about how it looks. It's about how it WORKS. Before a single pixel is placed, every user flow should be mapped, every decision point identified, and every friction point minimized.

## Step 1: User Flows

### Primary Flow (Critical Path)

The ONE flow that must be perfect. From "user lands" to "user gets value."

```
Example: Monitoring tool
Land on page → Click "Start free" → Enter email → 
Copy curl command → Run in terminal → See first data → 
"It works!"

Time to value: < 2 minutes
Number of steps: 5
Decision points: 0 (no choices required)
```

Rules:
- Count steps. Fewer = better. Each step loses ~20% of users.
- Count decision points. Each choice (plan, config, setting) creates friction.
- Identify the "aha moment" and place it as early as possible.

### Secondary Flows
2-3 other important flows. Same format, less detail.

### Error/Edge Case Flows
- What happens on wrong login?
- What happens if payment fails?
- What happens on network error?
- What does a user with an empty account see (empty state)?

Empty states are critical. They're often the first thing a new user sees. "No projects yet" vs "Create your first project in 30 seconds" is the difference between churn and activation.

## Step 2: Information Architecture

### Sitemap
```
Home
├── /app (logged in)
│   ├── Dashboard (landing after login)
│   ├── /projects
│   │   ├── Project list
│   │   └── /projects/:id (single project)
│   ├── /settings
│   │   ├── Profile
│   │   ├── Billing
│   │   └── Team
│   └── /docs
└── /marketing (logged out)
    ├── Landing page
    ├── Pricing
    └── Docs (public)
```

### Navigation
- **Primary nav:** Max 5 items. The most used things.
- **Secondary nav:** Settings, profile, help. Less visible.
- **Contextual nav:** Breadcrumbs, back buttons, sub-navigation per section.

Rule: A user should reach any page with max 3 clicks from dashboard.

## Step 3: Wireframe Notes

Instead of visual wireframes, write text wireframes:

```
PAGE: Dashboard
ABOVE FOLD:
  - Navigation (left sidebar, 64px collapsed)
  - Header with search + profile menu
  - Summary cards: [Metric 1] [Metric 2] [Metric 3]
  - Recent activity list (5 items, clickable)

BELOW FOLD:
  - Charts/graphs relevant to primary metric
  - Full item list with filters

PRIMARY ACTION: "Create new" button, top right
EMPTY STATE: "Nothing here yet. Create your first [thing] in 30 seconds."
MOBILE: Summary cards stack vertically, charts hidden below fold
```

## UX Principles

### Don't Make Me Think
Every time the user has to THINK about what the next step is, you've failed. The next step should be obvious.

### Progressive Disclosure
Don't show everything at once. Show what the user needs NOW. Show more when they ask. A settings page doesn't need 50 fields on first visit.

### Sane Defaults
Every setting should have a sensible default value. The user should be able to use the product without configuring ANYTHING. Configuration as a feature, not a requirement.

### Error Prevention > Error Handling
Better to prevent errors (disabled submit button until form is valid, inline validation, confirmation dialogs for destructive actions) than to handle errors after they occur.

### Mobile-First Flows
Design the flow for mobile FIRST. If it works on a 375px screen with one thumb, it works everywhere.
