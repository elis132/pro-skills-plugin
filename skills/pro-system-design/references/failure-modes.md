# Failure Modes: What Breaks at 3am

## The Mindset

A junior architect designs for the happy path. A senior architect designs for the 3am phone call. For every component in your system, you should be able to answer: "What happens when this stops working, and what do I do about it?"

## Universal Failure Modes

### The Database Dies
**What happens:** All requests fail. Data is inaccessible.
**Mitigations by level:**
- SQLite: Restart the application. If disk is corrupted, restore from backup. Keep Litestream running to S3 for continuous backups.
- Single Postgres: Restart the service. If data is corrupted, restore from WAL backup. Use `pg_basebackup` on a schedule.
- Postgres with replicas: Promote a replica. Application reconnects to new primary.
**Minimum viable plan:** Automated backups tested monthly (actually restore them, don't just check the backup exists). A runbook that says exactly what to do.

### Disk Fills Up
**What happens:** Writes fail. Logs stop. Database crashes. The system enters an unpredictable state.
**Why it happens:** Logs grow unbounded. Database WAL files accumulate. Temp files from processing. Uploaded files.
**Prevention:** Log rotation (logrotate on Linux). Disk space monitoring with alert at 80%. Maximum log size limits. Separate disk/partition for data vs logs.
**The rule:** If you're not monitoring disk space, your system WILL fill up eventually. It's not a question of if.

### Memory Leak
**What happens:** Application gets slower and slower, then OOM killer terminates it (on Linux) or it crashes.
**Why it happens:** Unbounded caches, connection pools that grow but don't shrink, event listener accumulation, large object retention.
**Prevention:** Set memory limits. Monitor RSS over time. Restart periodically if you can't find the leak (not elegant, but effective). In production, a process manager that auto-restarts is essential.

### External API Goes Slow
**What happens:** Your requests back up. Thread/connection pool fills. Your app becomes unresponsive to ALL requests, not just the ones using the slow API.
**Why it happens:** Third-party API is degraded. Network issues. DNS issues.
**Prevention:** Timeouts on EVERY external call (default: 5s connect, 10s read). Circuit breaker pattern for critical dependencies. Fallback behavior (serve cached data, degrade gracefully). Never let an external dependency take down your entire app.
**The timeout rule:** If you haven't set an explicit timeout, the default is usually infinity. That means one slow API call can hold a connection forever.

### Deploy Goes Wrong
**What happens:** New code breaks in production. Users see errors.
**Prevention by level:**
- Level 1: Can you roll back in under 60 seconds? (If not, fix this first)
- Level 2: Health check endpoint that the deploy system checks before routing traffic
- Level 3: Canary deploys (send 5% of traffic to new version, watch for errors)
- Level 4: Blue-green deployment (run both versions, switch traffic atomically)
**Minimum viable plan:** Every deploy should be reversible with one command. Test this regularly. "I'll just push a fix" is not a rollback plan.

### Secrets Leak
**What happens:** API keys, database credentials, or user data exposed.
**Why it happens:** Committed to git, logged in plaintext, exposed in error messages, hardcoded in client-side code.
**Prevention:** Environment variables for secrets, never in code. `.env` files in `.gitignore`. Secret scanning in CI. Audit error output for sensitive data. Rotate credentials regularly.

## Data Failure Modes

### Eventual Consistency Surprises
**What happens:** User writes data, immediately reads it, and gets stale data.
**When this occurs:** Read replicas with replication lag. Caching without invalidation. Distributed databases. CDN caching of API responses.
**Prevention:** Read-after-write from the primary. Cache invalidation on write. Be explicit about consistency guarantees in your API documentation.

### Migration Goes Wrong
**What happens:** Data loss, schema mismatch, application crashes.
**Prevention:** Always make migrations reversible (write both up AND down migration). Run migrations on a copy of production data first. Never drop columns/tables without a deprecation period. Expand-then-contract pattern: add the new column, deploy code that writes to both old and new, migrate data, deploy code that reads from new, then remove old.

### Backups Don't Work
**What happens:** You need to restore and discover your backup process has been silently failing for months.
**Prevention:** Test restores regularly (monthly minimum). Automate the test. Alert if backup hasn't completed. Store backups in a different region/provider than your primary data. Verify backup integrity, not just existence.
**The rule:** A backup you haven't restored from is not a backup. It's a hope.

## Scaling Failure Modes

### Thundering Herd
**What happens:** Cache expires, all requests simultaneously hit the database, database overwhelms.
**Prevention:** Cache stampede protection (lock/mutex on cache refresh). Jittered TTLs (don't expire all keys at the same time). Background cache refresh before expiry.

### N+1 Queries
**What happens:** Listing 100 items makes 101 database queries instead of 2. Page loads take 5 seconds.
**Prevention:** Use eager loading / JOINs. Monitor query count per request. Set an alert for requests that execute more than 20 queries.

### Connection Pool Exhaustion
**What happens:** Application can't get a database connection. All requests fail with timeout errors.
**Why it happens:** Long-running queries hold connections. Connection pool too small. Connection leak (opened but never returned). Serverless functions opening too many connections.
**Prevention:** Connection pool size matched to workload. Query timeouts. Connection leak detection. PgBouncer for serverless.

## The Minimum Viable Ops Checklist

For any production system, before launch:

- [ ] Automated backups (tested)
- [ ] Log aggregation (even just journald/logrotate is fine)
- [ ] Disk space monitoring with alert
- [ ] Health check endpoint
- [ ] One-command rollback
- [ ] Timeouts on all external calls
- [ ] Error tracking (Sentry, or even just structured logging)
- [ ] Uptime monitoring (even a free tier of any monitoring tool)
- [ ] Documented runbook: "what to do when X breaks"
- [ ] Secrets not in code

You don't need Grafana dashboards with 50 panels. You need to know when something breaks and have a plan for fixing it.
