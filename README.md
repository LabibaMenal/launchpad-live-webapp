# Launchpad

**From "I have an app" to a live, verified site, in one place.**

Live prototype: https://launchpad-demo1.netlify.app/

## The problem
AI tools can now generate a working web app from a plain-language description. Getting that app *online* is still where most people stall. The generated output is source code or a preview, not a running service, and turning it into one needs several separate operational steps:

- **Build and runtime detection:** framework, output directory and Node version are guessed by hand, and failures show up as raw logs.
- **Hosting and release:** preview URLs can be ephemeral or private, and a successful deploy does not guarantee the site is publicly reachable.
- **DNS and TLS:** record types, propagation delay and certificate issuance fail silently or slowly.
- **Forms:** static pages have no server-side sink. A `mailto:` form looks correct in the browser but delivers nothing and stores nothing.
- **Updates and rollback:** edits overwrite production, with no safe way to undo a mistake.
- **Readiness:** nothing tells you whether the site is actually ready for real visitors after it goes live.

## The idea
One deployment engine (ingest, build, release, verify) with three entry points:

1. **AI-assisted creation:** describe the app, get a deployable project.
2. **One-click hosting:** bring existing code and publish it.
3. **Managed delivery:** hand the whole job to someone else.

A **Launch Score** readiness probe verifies the result instead of assuming it.

## What this prototype contains
- Landing page with the three entry lanes
- Launch Score: loads, HTTPS, response time, custom domain
- Simulated build log, DNS check, forms inbox and version rollback

### Real vs simulated
- **Real:** the deployment on Netlify, the live site, Launch Score reachability and timing checks, form capture, rollback via published deploys
- **Simulated in the page:** build log, DNS check, email delivery
- **Out of scope:** domain registrar, payment automation, Kubernetes/CDN

## Proposed production architecture
Node/Express API with streamed build logs, a job queue, a rootless Docker builder with resource quotas, a content-addressed immutable artifact store with an atomic `current` pointer (rollback is a pointer flip), a Caddy edge with on-demand TLS, ownership-verified custom domains, and a forms endpoint with honeypot, rate limiting and a retrying email outbox.

## Run locally
Open `index.html` in a browser. No build step.
