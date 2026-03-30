# Implementation Phase: Build What Was Planned

## Principle

You build EXACTLY what phases 1-4 specified. Not more. Not differently. If you discover the spec doesn't work in practice, GO BACK and update the spec. Don't hack around it.

## Build Order

### Always backend-first
1. Data model + migrations
2. Auth (registration, login, session)
3. CRUD endpoints for core entities
4. Background processes (if applicable)
5. Frontend: layout + navigation
6. Frontend: primary flow (critical path from phase 3)
7. Frontend: secondary flows
8. Frontend: error states + empty states
9. Polish: loading states, transitions, responsiveness

### Why this order
- Backend-first: you can test the API with curl before frontend exists
- Auth early: almost everything else depends on it
- Primary flow first: most important functionality tested early
- Polish last: if time runs out you have a working MVP without polish. Never a polished app that doesn't work.

## Code Principles

### One file per concept
```
# Good
routes/projects.py
routes/users.py
services/monitoring.py

# Bad
routes.py (1500 lines with everything)
```

### Consistent naming
Pick conventions and follow them:
- `snake_case` for Python, filenames, database columns
- `camelCase` for JavaScript variables and functions
- `PascalCase` for React components and classes
- `SCREAMING_SNAKE` for constants and env variables

### Error handling
Every external call (database, API, filesystem) can fail. Handle it:
```python
try:
    result = await external_api.call()
except TimeoutError:
    return error_response("External service timed out", 503)
except Exception as e:
    logger.error(f"Unexpected error: {e}")
    return error_response("Internal error", 500)
```

### Environment variables
All secrets, URLs, and configuration that varies between environments. Never hardcoded. Never in git.

## File Structure (per stack)

### Python/FastAPI
```
project/
├── main.py              # App entry, route registration
├── config.py            # Settings from env vars
├── database.py          # DB connection, session
├── models/              # SQLAlchemy/Pydantic models
├── routes/              # API endpoints by resource
├── services/            # Business logic
├── tasks/               # Background jobs
├── static/              # CSS, JS, images
├── migrations/          # Alembic migrations
├── tests/
└── requirements.txt
```

### Next.js (App Router)
```
project/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── (auth)/          # Auth route group
│   ├── (app)/           # Authenticated route group
│   │   ├── dashboard/
│   │   └── projects/
│   └── api/             # API routes
├── components/
├── lib/                 # Utilities, DB client, auth
├── prisma/
├── public/
└── .env.local
```

## Implementation Rules

### Rule 1: Working > Perfect
V1 doesn't need to be beautiful code. It needs to work. Refactoring is v2.

### Rule 2: One feature, test, next feature
Don't build three features in parallel. Build one, verify it works, then the next. You catch problems early and avoid debugging three things simultaneously.

### Rule 3: Use what you know
If you know Python, build in Python. Learning a new framework mid-project delays everything. New tech = separate learning sprint, not production project.

### Rule 4: No TODOs in shipped code
If there's a TODO, either fix it now or put it in an issue tracker. TODO comments in code die where they're born.

## Scope Guard

During implementation, the brain wants to add things. "Oh, we should have an admin panel." "We should have dark mode." "We should have CSV export."

The answer: "Is this in the phase 1 scope?"
- Yes → build it
- No → write it on a v2 list, do NOT build it now

This is the single most important discipline in solo development. Scope creep kills more projects than bad code.
