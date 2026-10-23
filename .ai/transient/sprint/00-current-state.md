# Current Sprint State: Inception & Foundation

> **Sprint:** Sprint 01 â€” Demos Platform Inception & Accounts Demo Setup  
> **Status:** In Progress  
> **Target Date:** 2026-10-23  

---

## 1. Sprint Focus & Goals

The primary objective of this sprint is to initialize the multi-app demo hosting platform at `demos.alamiasoft.com` and prepare the first production-grade demo stack for **Alamia Accounts** targeting Mr. Muzaffar's evaluation.

### Core Deliverables
1. **Repository Foundation & Lifecycle Integration:**
   - Integrate `Alamia-Skills-System` as git submodule and bootstrap lifecycle skills.
   - Establish canonical AI Knowledge Base in `.ai/` preserving all decisions from `docs/00-start.md`.
2. **Central Portal Application (`demos.alamiasoft.com`):**
   - Lightweight, responsive landing portal showcasing the Alamia product portfolio.
   - Professional styling, status indicators, links to active demos, and client instructions.
   - Dockerfile and Nginx configuration with anti-crawling headers.
3. **Alamia Accounts Demo Stack (`stacks/accounts/`):**
   - Independent Docker Compose definition for Alamia Accounts.
   - Environment templates (`.env.example`) and volume definitions.
   - Seeding and setup documentation for Mr. Muzaffar's demonstration account.
4. **Cloudflare Tunnel Routing Configuration:**
   - Reference `config.yml` demonstrating multi-ingress routing for all `*.demos.alamiasoft.com` subdomains to internal Docker services.

---

## 2. Active Working Tasks

- [x] Initialized Git repository and linked `Alamia-Skills-System` submodule.
- [x] Bootstrapped lifecycle skills (`bootstrap`, `implement`, `qa`, `architecture-review`, `session-end`).
- [x] Codified ADR-0001, system architecture, standards, glossary, and dependency maps in `.ai/`.
- [x] Implemented Central Demo Portal application in `demos.alamiasoft.com/` (index.html, nginx.conf with noindex, Dockerfile, docker-compose.yml).
- [x] Deployed and verified Central Demo Portal live on VPS at `https://demos.alamiasoft.com`.
- [x] Resolved Portainer deployment configuration for Alamia Accounts (standalone build, `/alamia-accounts` slug preserved, port 8101).
- [x] Added `PHP_CLI_SERVER_WORKERS: 8` to eliminate 522 concurrency bottlenecks in Accounts backend.
- [ ] Redeploy Alamia Accounts stack in Portainer and verify fast UI loading on `https://accounts-demo.alamiasoft.com/alamia-accounts`.
- [ ] Verify test login and review synthetic financial ledgers for Mr. Muzaffar.

---

## 3. Known Blockers & Invariants

- **Blocker:** None currently.
- **Invariants to Guard:** Never mix container runtimes or databases across stacks. No real customer data allowed.
