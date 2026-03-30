# Testing Phase: Try to Break It

## Principle

Your job as QA is not to confirm it works. Your job is to find ways to BREAK it. Think like a malicious user, a confused beginner, and a hacker. Simultaneously.

## Test Priority

### Level 1: Primary Flow (Critical Path)
Test every step of the primary flow from phase 3. If THIS doesn't work, nothing else matters.
- Normal flow (happy path)
- What happens if you skip a step?
- What happens if you go back mid-flow?
- What happens if you do the same step twice?

### Level 2: Auth and Data
- Can you access other users' data by changing the URL/ID?
- Can you make API calls without being logged in?
- Does logout work properly (session invalidated)?
- What happens when the session expires mid-flow?

### Level 3: Input Validation
Every field that accepts input:
- Empty input
- Extremely long input (10,000 characters)
- Special characters (', ", <, >, &, ;, --)
- Numbers in text fields, text in number fields
- Negative numbers, zero, decimals
- Emojis (yes, really. They crash things surprisingly often.)
- SQL injection: `' OR 1=1 --`
- XSS: `<script>alert('xss')</script>`

### Level 4: Edge Cases
- What happens with 0 items? (Empty state)
- What happens with 1 item? (Singular/plural)
- What happens with 1000 items? (Performance, pagination)
- What happens on rapid double-click of submit?
- What happens on network failure mid-request?
- What happens if you open the same page in two tabs?

### Level 5: Responsiveness
- iPhone SE (375px)
- iPhone 14/15 (390px)
- iPad (768px)
- Laptop (1366px)
- Desktop (1920px)

Check: does text clip? Do elements overlap? Are touch targets at least 44px?

## Bug Severity

```
CRITICAL: App crashes, data is lost, security hole
          → Must fix BEFORE launch

HIGH:     Primary flow blocked, payment broken,
          data displayed incorrectly
          → Should fix before launch

MEDIUM:   Secondary flow affected, visual bug that confuses,
          poor performance on specific pages
          → Fix after launch ok, but soon

LOW:      Cosmetic bug, minor inconsistency, works but
          looks slightly off
          → Backlog
```

## Test Report Format

```
TESTED VERSION: [date, commit hash if applicable]
TESTED BY: [QA hat]
TEST ENVIRONMENT: [Browser + OS + viewport]

CRITICAL FLOW: [Pass/Fail with details]

BUGS:
#1 [CRITICAL] - [Short description]
   Steps to reproduce: [1, 2, 3]
   Expected: [What should happen]
   Actual: [What happens]
   
#2 [HIGH] - ...

APPROVED FOR LAUNCH: [Yes/No + conditions]
```

## Performance Test (Basic)

- Page load time: under 3 seconds?
- API response time: under 500ms for common queries?
- Does it work with 100 records? 1,000? 10,000?
- Are there N+1 queries? (Check network tab: does the page make 50 API calls for a list?)

## Pre-Launch Checklist

- [ ] Primary flow works on desktop AND mobile
- [ ] Auth works (registration, login, logout, session timeout)
- [ ] No known CRITICAL or HIGH bugs
- [ ] Error messages are helpful (not "Error 500")
- [ ] Empty states designed (not just blank pages)
- [ ] Form validation works
- [ ] Responsive on at least 3 sizes (mobile, tablet, desktop)
- [ ] No console.errors in browser
