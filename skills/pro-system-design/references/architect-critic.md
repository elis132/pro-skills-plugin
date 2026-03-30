# Architect Critic: Self-Review Rubric

## The Napkin Test

Can you draw the entire system on a napkin? If not, it's too complex for the current stage. A system that can't be explained in one diagram probably can't be debugged at 3am either.

## Over-Engineering Check

For EACH component in your design, answer:

**"What specific, current problem does this solve?"**

If the answer starts with "in case we need to..." or "when we scale to..." or "for future flexibility..." then you're over-engineering. Remove it. Add it when you actually need it.

Specific red flags:

- [ ] **Microservices for a team of 1-3.** Unless each service has genuinely different scaling needs (one needs GPUs, one doesn't), this is over-engineering.
- [ ] **Kubernetes for fewer than 5 services.** Docker Compose on a single host, or just systemd services, will be simpler.
- [ ] **Redis when Postgres handles the load.** Have you actually measured that Postgres is too slow? What's the actual query time?
- [ ] **Message queue when synchronous works.** Does the task actually need to be async? Can it complete in <200ms? Then do it in the request.
- [ ] **Multiple databases for different data types.** Unless you have a specific workload that Postgres genuinely can't handle (high-write time-series, full-text search at huge scale), one database is simpler.
- [ ] **Event-driven architecture for CRUD.** If your app is create/read/update/delete with a web frontend, request-response is fine.
- [ ] **API gateway for one service.** You don't need Kong or Traefik routing to a single backend. Your web framework handles routing.
- [ ] **Separate auth service for one application.** Use a library (Passport, NextAuth, etc.) until you have multiple applications sharing auth.

## Under-Engineering Check

The opposite problem. Red flags:

- [ ] **No backups.** For any production data.
- [ ] **No health checks.** How do you know it's working?
- [ ] **No timeouts on external calls.** One slow API call will eventually hang your entire application.
- [ ] **No error tracking.** "Nobody reported a bug" doesn't mean there are no bugs.
- [ ] **Hardcoded secrets.** In code, in docker-compose.yml, in client-side bundles.
- [ ] **No rollback plan.** "I'll just push a fix" is not a plan.
- [ ] **No rate limiting on public endpoints.** One abusive client can take down your service.
- [ ] **SQL injection possible.** Using string concatenation for queries instead of parameterized queries.

## The Resume-Driven Development Check

Be honest: is any technology in this design chosen because it would look good on a resume rather than because it's the right tool? Common offenders:

- Kubernetes (when Docker Compose or systemd works fine)
- GraphQL (when REST serves one frontend)
- Microservices (when a monolith would be simpler)
- Kafka (when Postgres LISTEN/NOTIFY or a simple job table works)
- Terraform (when clicking in the AWS console or a simple deploy script works)
- Rust (when Python or Go would ship faster and performance isn't the bottleneck)

There's nothing wrong with any of these technologies. They're all excellent tools. But using them when simpler alternatives work is a tax on every future hour of development.

## The 3am Test

For each component, honestly answer: "If this breaks at 3am, can I fix it?"

- Can you access the system remotely?
- Do you know where the logs are?
- Can you restart the service?
- Can you roll back the last deploy?
- Can you restore from backup?
- Do you understand the technology well enough to debug it under stress?

If the answer to any of these is "no" for a component you've chosen, either learn it properly before using it in production, or choose something you already know.

## The Cost Test

Calculate the actual monthly cost:

- Hosting/compute
- Database (managed vs self-hosted)
- Third-party services (monitoring, error tracking, email, SMS)
- CDN
- Domain/SSL (usually free with Let's Encrypt)

For a small SaaS with <100 paying customers, the total should be under $50/month. If your infrastructure costs more than your revenue for the first year, the architecture is wrong.

## The "What Would I Cut?" Test

If you had to remove ONE component from the design, which would it be? Remove it. Does the system still work? If yes, you didn't need it.

Keep doing this until removing anything would break the system. That's your minimum viable architecture.

## Comparison Check

If this isn't the first system you've designed in this conversation, compare:

| Dimension | This system | Previous system | Same? |
|---|---|---|---|
| Database choice | ? | ? | ? |
| Deployment approach | ? | ? | ? |
| Architecture style | ? | ? | ? |
| Language/framework | ? | ? | ? |

If everything is the same, you might be defaulting to "your stack" instead of "the right stack for this problem." Different problems often deserve different solutions.
