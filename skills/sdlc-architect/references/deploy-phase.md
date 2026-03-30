# Deploy Phase: Ship It

## Principle

Deployment complexity should match project complexity. A solo SaaS doesn't need Kubernetes. A `git push` with auto-deploy is often enough.

## Deployment Strategies by Level

### Level 1: Simplest possible (solo project)
```
Local dev → git push → SSH to server → git pull → restart
```

### Level 2: PaaS auto-deploy (recommended for most)
```
git push main → Railway/Render/Vercel builds automatically → live
```
Zero ops, automatic SSL, rollback with one click. $5-25/mo.

### Level 3: VPS with simple CI/CD
```
git push main → GitHub Action → SSH to server → pull + build + restart
```
More control, lower cost. ~2 hours to set up, then automatic.

### Level 4: Docker Compose (multiple services on one server)
```
git push → CI builds Docker images → docker compose up -d
```
For: app + worker + database + redis on the same machine.

## Launch Checklist

### Infrastructure
- [ ] HTTPS active (Caddy auto-TLS, Let's Encrypt, or Cloudflare)
- [ ] Domain pointing correctly (DNS propagated, verified)
- [ ] Environment variables set in production
- [ ] Database migrations run
- [ ] Backup schedule active and TESTED (have you actually done a restore?)

### Security
- [ ] Secrets not in git (check history, not just latest commit)
- [ ] CORS configured correctly (not `*` in production with auth)
- [ ] Rate limiting on auth endpoints
- [ ] HTTP security headers set
- [ ] Input sanitization in backend (never trust frontend validation)

### Monitoring
- [ ] Uptime monitoring configured (UptimeRobot free tier works)
- [ ] Error tracking configured (Sentry free tier works)
- [ ] Basic logging that persists at least 7 days
- [ ] Alert channel configured (email, Slack, SMS)

### Rollback
- [ ] You can roll back to previous version in < 60 seconds
- [ ] You have TESTED the rollback process
- [ ] Database migrations are reversible

### Analytics
- [ ] Visitor tracking installed (Plausible, Fathom, or GA4)
- [ ] Conversion events tracked (signup, activation, payment)
- [ ] Error rates visible

## Post-Launch (Day 1-7)

### Day 1: Watch mode
- Check error tracking every 30 minutes
- Check uptime monitoring
- Verify signups/payments work
- Be ready to roll back

### Day 2-3: Fix critical issues
- Fix bugs that real users find
- Check performance under real traffic
- Update FAQ/docs based on first user questions

### Day 4-7: Stabilize
- Check retention (are people coming back?)
- Check funnel (where are you losing people?)
- Collect feedback (not feature requests, but frustrations)

## Simplest CI/CD Setup (GitHub Actions)

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to server
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USER }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /opt/myapp
            git pull origin main
            pip install -r requirements.txt --break-system-packages
            alembic upgrade head
            sudo systemctl restart myapp
```

12 lines. Entire CI/CD pipeline. Pushes to main deploy automatically.

## What You DON'T Need (Day 1)

- Kubernetes
- Terraform
- AWS/GCP/Azure (a VPS is enough)
- Load balancer (one server is enough)
- CDN (until you have international traffic)
- Staging environment (until you have paying customers affected by bugs)
- Blue-green deployment (until you have zero-downtime as a requirement)

Add these when you NEED them. Not before.
