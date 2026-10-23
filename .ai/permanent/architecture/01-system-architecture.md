# System Architecture & Technical Specification

> **Platform:** AlamiaSoft Demos Multi-Tenant Demonstration Infrastructure  
> **Primary Domain:** `demos.alamiasoft.com`  
> **Status:** Active / Foundational  

---

## 1. High-Level System Overview

The AlamiaSoft Demos Platform provides a centralized, secure, and isolated hosting environment for demonstrating the suite of Alamia software products (e.g., Alamia Accounts, TravelOS, AlamiaConnect CRM) to prospective clients, investors, and stakeholders (including the initial target demonstration for Mr. Muzaffar).

### Core Architectural Principles
1. **Unified Root Namespace, Strict Subdomain Isolation:** All product demonstrations are hosted under dedicated subdomains of `demos.alamiasoft.com` (e.g. `accounts.demos.alamiasoft.com`), sharing a common brand namespace while maintaining total application autonomy.
2. **Zero Runtime & Persistence Sharing:** A shared domain and shared VPS do **not** imply a shared database or application process. Each product is encapsulated in its own Docker stack with independent databases, file volumes, caches, and environment configurations.
3. **Synthetic Data Exclusivity:** Absolutely no real-world financial, customer, or confidential operational data is permitted in any demo environment. All demo data must be seeded through deterministic synthetic fixtures.
4. **Edge Security & Stealth Ingress:** Demo instances are not exposed directly to the public internet via open VPS ports. All ingress traffic is funneled through Cloudflare Tunnels (HTTPS only), enforced with bot prevention, `noindex` headers, and Cloudflare Access / token-gated boundaries where appropriate.

---

## 2. Ingress & Routing Topology

```mermaid
flowchart TD
    subgraph Public Internet / Clients
        ClientMuzaffar["Stakeholder / Client (Mr. Muzaffar)"]
        PublicVisitors["Prospective Leads & Evaluators"]
    end

    subgraph Cloudflare Edge
        DNS["Cloudflare DNS (*.demos.alamiasoft.com)"]
        CF_Access["Cloudflare Access / WAF Rules"]
        CF_Tunnel["Cloudflare Tunnel (cloudflared)"]
        DNS --> CF_Access
        CF_Access --> CF_Tunnel
    end

    subgraph Host Infrastructure (Single VPS / Ubuntu + Docker)
        CloudflaredAgent["Cloudflare Tunnel Daemon: 'alamiaconnect-vps-01'<br/>ID: 046f1c34-6e56-401a-b871-1a82238c4baf"]
        DockerNetwork["Internal Docker Bridge Network(s)"]
        Portainer["Portainer CE (Stack Management)"]
        
        CF_Tunnel -. Encrypted Ingress .-> CloudflaredAgent
        CloudflaredAgent --> DockerNetwork
    end

    subgraph Stacks [Independent Docker Compose Stacks]
        subgraph PortalStack [Portal Stack: demos.alamiasoft.com]
            PortalApp["Central Portal Web App (Landing / Directory)"]
        end

        subgraph AccountsStack [Stack: accounts.demos.alamiasoft.com]
            AccountsApp["Alamia Accounts App Container"]
            AccountsDB[(PostgreSQL / MySQL - Accounts Synthetic DB)]
            AccountsApp --- AccountsDB
        end

        subgraph TravelOSStack [Stack: travelos.demos.alamiasoft.com (Future)]
            TravelOSApp["TravelOS App Container"]
            TravelOSDB[(TravelOS Dedicated DB)]
            TravelOSApp --- TravelOSDB
        end

        subgraph ConnectStack [Stack: connect.demos.alamiasoft.com (Future)]
            ConnectApp["AlamiaConnect CRM App Container"]
            ConnectDB[(Connect Dedicated DB)]
            ConnectApp --- ConnectDB
        end
    end

    ClientMuzaffar --> DNS
    PublicVisitors --> DNS
    DockerNetwork --> PortalApp
    DockerNetwork --> AccountsApp
    DockerNetwork --> TravelOSApp
    DockerNetwork --> ConnectApp
```

---

## 3. Subdomain Registry & Intent

| Domain / FQDN | Role | Target Application | Initial Priority | Access Controls |
| :--- | :--- | :--- | :--- | :--- |
| `demos.alamiasoft.com` | Central Demo Portal | Static/SSR Catalog of Live Demos | High | Public / Noindex |
| `accounts.demos.alamiasoft.com` | Product Demo | Alamia Accounts (`https://github.com/AlamiaSoft/AlamiaAccounts`) | **Priority 1 (Target: Mr. Muzaffar)** | Dedicated Demo User Account |
| `travelos.demos.alamiasoft.com` | Product Demo | TravelOS (Travel ERP) | Phase 2 | Dedicated Demo User Account |
| `connect.demos.alamiasoft.com` | Product Demo | AlamiaConnect CRM | Phase 3 | Dedicated Demo User Account |

---

## 4. Architectural Rules & Invariants

### Invariant 1: Complete Persistence Isolation
* No container in Stack A may connect to the database, Redis cache, or storage volume of Stack B.
* Each demo stack defines its own named networks or connects to an ingress network solely for reverse proxying.
* Passwords, encryption keys (`APP_KEY`), and database credentials must be completely decoupled across stacks.

### Invariant 2: Synthetic Data Hygiene
* Every demo database must be populated exclusively using automated seeders producing synthetic companies, simulated invoices, fictitious ledger accounts, and dummy transactions.
* Database dumps from production or staging environments containing real customer or entity data are **strictly forbidden**.

### Invariant 3: Search Engine Anti-Indexing
* All HTTP responses across `*.demos.alamiasoft.com` must emit:
  ```http
  X-Robots-Tag: noindex, nofollow, noarchive, nosnippet
  ```
* Every web root must serve a standard `robots.txt` disallowing all user agents:
  ```txt
  User-agent: *
  Disallow: /
  ```

### Invariant 4: Blast Radius Containment & Independent Deployability
* Any demo stack may be torn down (`docker compose down -v`), rebuilt, or modified without causing downtime or state mutations in any other demo stack or the central portal.
* Resource quotas (CPU limits, memory limits) must be declared in Docker Compose to prevent any single demo stack experiencing runaway queries from starving adjacent demos.

---

## 5. Failure Modes & Mitigations

| Failure Mode | Trigger / Vector | Architectural Mitigation |
| :--- | :--- | :--- |
| **Demo Data Corruption** | Client or evaluator performs destructive actions or alters demo records. | Automated nightly or on-demand reset scripts re-running migrations and synthetic seeders. |
| **Resource Starvation** | Heavy reporting query run on Alamia Accounts demo exhausts host RAM. | Docker Compose `deploy.resources.limits.memory` configured on every container (e.g. 512MB-1GB limits). |
| **Accidental Data Leak** | Developer dumps real client backup into demo instance. | Enforced synthetic seeder CI validation and code review checks; forbid production DB credential storage on VPS. |
| **Tunnel Connection Drop** | Host network interruption or cloudflared daemon crash. | Cloudflared run as a restartable Docker service (`restart: always`) with multiple replica ingress routes or health monitors. |

---

## 6. Tradeoffs & Rejected Alternatives

1. **Shared Multi-Tenant Database vs. Isolated Stacks:**
   - *Rejected:* Running all demo apps against a single PostgreSQL/MySQL server with shared users or schemas.
   - *Rationale:* While saving a few hundred megabytes of RAM, a shared database creates a single point of failure, complicates individual version upgrades, and violates the zero-leak blast radius invariant.
2. **Direct Port Exposure vs. Cloudflare Tunnel:**
   - *Rejected:* Exposing ports 80/443 on the VPS host with Certbot / Let's Encrypt.
   - *Rationale:* Cloudflare Tunnel prevents the VPS IP from being discovered or port-scanned, centralizes SSL/TLS lifecycle, and enables instant DNS routing updates without touching host firewall rules.
