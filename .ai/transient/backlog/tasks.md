# Project Backlog & Task Queue

---

## High Priority: Foundation & First Demo

- [ ] **TASK-001: Central Demo Portal App**
  - Path: `demos.alamiasoft.com/`
  - Scope: Clean, modern landing page listing product demos (Accounts, TravelOS, Connect) with status badges, links, and evaluation notes. Includes container configuration and Nginx anti-indexing rules.
- [ ] **TASK-002: Alamia Accounts Demo Stack**
  - Path: `stacks/accounts/`
  - Scope: Production-ready Docker Compose stack with isolated app and database services, `.env.example`, persistent named volumes, and synthetic seeding notes for Mr. Muzaffar's evaluation.
- [ ] **TASK-003: Ingress & Cloudflare Tunnel Reference**
  - Path: `ingress/` or `stacks/tunnel/`
  - Scope: Reference `cloudflared` configuration mapping `demos.alamiasoft.com` and `accounts.demos.alamiasoft.com` to the internal Docker network.

## Medium Priority: Expansion

- [ ] **TASK-004: TravelOS Demo Stack Skeleton**
  - Path: `stacks/travelos/`
  - Scope: Scaffold stack configuration ready for TravelOS demo rollout.
- [ ] **TASK-005: Automated Synthetic Data Reset Script**
  - Scope: Helper utility to reset demo databases to a clean synthetic baseline on demand.
