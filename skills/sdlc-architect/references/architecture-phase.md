# Architecture Phase: Technical Decisions That Hold

## Principles

The architecture should be the SIMPLEST thing that meets the requirements from the product brief. Not the "best." Not the "most modern." The simplest.

Every technology you add is a technology you must maintain, upgrade, debug, and explain. Minimize moving parts.

## Stack Selection Based on Product Brief

### Solo-developer SaaS (1 person, bootstrapped)
```
Runtime:  Python/FastAPI OR Next.js
Database: SQLite (Litestream for backup) OR Postgres via Supabase
Auth:     Supabase Auth OR NextAuth OR custom JWT
Hosting:  Hetzner VPS ($5-10/mo) OR Vercel/Railway
```
Rationale: minimal number of technologies, one server, one deploy, one log stream.

### Team project (2-5 developers)
```
Runtime:  Next.js OR FastAPI + React
Database: PostgreSQL (managed: Supabase, Neon, or Railway)
Auth:     Supabase Auth OR Auth.js
Queue:    Postgres-based (pgboss, BullMQ with Redis)
Hosting:  Railway/Render OR Hetzner + Docker Compose
```
Rationale: more people require clearer separation, managed database reduces ops burden.

### Product with real-time (chat, live updates, collaboration)
```
Add:    WebSockets OR SSE (Server-Sent Events)
Consider: Redis pub/sub (if multiple servers)
Avoid:  Polling (works but creates unnecessary load)
```

### Product with heavy background processing (AI, image generation, data analysis)
```
Add:    Background worker (separate process)
Queue:  BullMQ (Node) OR Celery (Python) OR Postgres job table
Consider: Separate resources for worker vs web
```

## Data Modeling

### Step 1: Identify entities
From the product brief, list all "things" in the system.

### Step 2: Define relationships
- User has many Projects (1:N)
- Project has many Tasks (1:N)
- User belongs to Organization (N:1)

### Step 3: Define attributes
Only what the MVP needs. Not all future fields.

### Step 4: Indexing strategy
Which queries will run most often? Index those columns.

## API Design (Quick Guide)

### URL structure
```
GET    /api/projects          → List
POST   /api/projects          → Create
GET    /api/projects/:id      → Get one
PATCH  /api/projects/:id      → Update
DELETE /api/projects/:id      → Delete
GET    /api/projects/:id/tasks → Nested resource
```

### Consistent response format
```json
{ "data": { ... }, "error": null }
{ "data": null, "error": { "code": "NOT_FOUND", "message": "..." } }
```

### Auth
Every non-public endpoint requires auth. Decide early:
- Cookies (good for web, simpler CSRF handling)
- Bearer token (good for API, mobile, third-party integrations)
- API keys (good for machine-to-machine)

## Architecture Decision Format

Every decision gets documented:

```
DECISION: Using SQLite instead of PostgreSQL
CONTEXT: Solo developer, one server, <1000 users expected year 1
ALTERNATIVE: PostgreSQL (managed via Supabase)
RATIONALE: Simpler ops, no network latency, backup = copy file
RISK: If we need multi-server it won't scale
MIGRATION PLAN: SQL syntax is compatible, migration takes ~1 day
```

Always document the migration plan. All decisions are temporary. The question is how expensive it is to change your mind.
