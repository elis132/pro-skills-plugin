# Code Critic: Review Your Own Code

## The Anti-Pattern Scan

Go through each item. If more than 2 are present, do another editing pass.

### Structure
- [ ] No file exceeds 200 lines
- [ ] No function exceeds 40 lines
- [ ] No function has more than 3 parameters (use object for more)
- [ ] Files are organized by feature, not by type
- [ ] Each function does ONE thing (can you name it clearly?)
- [ ] No god-objects or god-components that do everything

### Error Handling
- [ ] No `catch (e) { console.error(e) }` that swallows errors silently
- [ ] Every database query handles null/not-found
- [ ] Every external API call has a timeout
- [ ] Every form submit is protected against double-click
- [ ] Error messages are user-facing and actionable (not "Error" or stack traces)
- [ ] ONE consistent error strategy (throw+catch OR result types, not mixed)

### AI Tells
- [ ] No over-abstraction (interfaces/classes for things with only one implementation)
- [ ] No TODO/FIXME/HACK comments (either fix it or remove the feature)
- [ ] No unused imports, variables, or dead code
- [ ] No `any` types in TypeScript (use proper types or `unknown`)
- [ ] No `console.log` debugging left in production code
- [ ] No commented-out code blocks (use git, not comments)

### Defensive Coding
- [ ] Input validated at API boundaries (not deep inside business logic)
- [ ] No `.property` access on potentially null values without null check
- [ ] Race conditions considered for concurrent operations (double-submit, booking conflicts)
- [ ] Sensitive data not logged (passwords, tokens, personal data)

### Data Access
- [ ] No N+1 queries (no database calls inside loops)
- [ ] Pagination on every list endpoint
- [ ] Only needed fields selected (not `SELECT *`)
- [ ] Indexes exist for frequent query patterns
- [ ] Transactions used for multi-record operations that must be atomic

### React (if applicable)
- [ ] No unnecessary useEffect (derived state computed directly, events in handlers)
- [ ] Loading, error, and empty states handled for every async operation
- [ ] Forms have validation, disabled-during-submit, and error display
- [ ] No prop drilling more than 2 levels (use context or composition)
- [ ] Components under 80 lines (extract sub-components if longer)

## The Read-Aloud Test (for code)

Read each function name aloud as a sentence:
- `createBooking` — "Create booking." Clear.
- `handleData` — "Handle data." Handle WHAT data? Rename.
- `process` — "Process." Process WHAT? Rename.
- `doStuff` — Absolutely not.

Read each file's import list. Can you tell what the file does from its imports alone? If not, the file is doing too much or the imports are poorly organized.

## The Delete Test

For each abstraction layer (interface, base class, service wrapper, utility function):
- What happens if you delete it and inline the code?
- Is the code simpler or more complex?
- If simpler: delete the abstraction. It wasn't earning its keep.

## The New Developer Test

Imagine a developer who has never seen this codebase opens your PR:
- Can they understand what each file does from the filename?
- Can they understand what each function does from the name?
- Can they follow the flow from API route to database and back?
- Are there any "magic" values or behaviors that aren't explained by a comment?

If any answer is "no," the code needs more clarity (better names, a comment explaining WHY, or a simpler structure).

## The 3am Test

You get paged at 3am because this code threw an error in production:
- Can you find the relevant file in under 30 seconds?
- Does the error message tell you what went wrong?
- Can you read the function and understand it while half-asleep?
- Is there a clear path from error to fix?

Code that passes the 3am test is code worth shipping.
