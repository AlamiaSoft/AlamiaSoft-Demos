# ADR 0001: Central Demos Subdomain Namespace & Isolated Docker Stacks

> **Status:** Accepted  
> **Date:** 2026-10-23  
> **Deciders:** Alamia Architecture Team  
> **Source Reference:** `docs/00-start.md`  

---

## Context & Problem Statement

AlamiaSoft develops multiple software solutions across different domains, including **Alamia Accounts** (accounting & financial management), **TravelOS** (travel industry ERP), and **AlamiaConnect** (CRM & communications). 

Prospective clients and stakeholders (beginning immediately with Mr. Muzaffar for Alamia Accounts) require live, interactive demonstration environments. We need a hosting strategy that:
1. Presents a cohesive, professional brand identity.
2. Avoids coupling unrelated product demos into a brittle shared application runtime.
3. Keeps deployment simple and cost-effective initially (single VPS).
4. Enables independent deployment, teardown, and reset of individual product demos.

## Decision Drivers

- **Brand Neutrality:** Demos span different products; the root namespace should belong to the umbrella parent (`alamiasoft.com`), while leaving product-specific demo domains (such as `demos.alamiaconnect.com`) reserved if required later.
- **Data Safety:** Client demo instances must never contain real financial or business data.
- **Operational Simplicity:** A single shared VPS managed with Docker and Portainer minimizes initial infrastructure overhead.
- **Blast Radius Containment:** A crash, schema update, or memory spike in one product demo must never disrupt another product demo or the central portal.

---

## Considered Alternatives

1. **Alternative 1: Product-Specific Demo Subdomains (e.g. `demos.alamia-accounts.com`, `demos.alamiaconnect.com`)**
   - *Downside:* Fragments branding, requires separate SSL certificates and DNS zones across disparate domains, and lacks a unified showcase portal.
2. **Alternative 2: Single Monolithic Docker Stack with Shared Database**
   - *Downside:* Violates tenant boundary isolation. Upgrades in one app risk destabilizing shared database services.
3. **Alternative 3: `demos.alamiasoft.com` Namespace with Isolated Docker Stacks on Shared VPS (Chosen)**
   - Use `demos.alamiasoft.com` as the central portal.
   - Host individual product demos as sub-subdomains (`accounts.demos.alamiasoft.com`, `travelos.demos.alamiasoft.com`).
   - Run each demo as an independent Docker Compose stack with its own database and persistence volumes.
   - Route traffic securely using Cloudflare Tunnels.

---

## Decision Outcome

**Chosen Option:** Alternative 3 (`demos.alamiasoft.com` with isolated Docker stacks via Cloudflare Tunnel).

### Key Rules Accepted:
1. **Domain Namespace:**
   - `demos.alamiasoft.com` â†’ Central demo portal & shared landing catalog.
   - `accounts.demos.alamiasoft.com` â†’ Alamia Accounts demo (Priority 1 for Mr. Muzaffar).
   - `travelos.demos.alamiasoft.com` â†’ TravelOS demo.
   - `connect.demos.alamiasoft.com` â†’ AlamiaConnect CRM demo.
2. **Deployment Architecture:**
   - Single VPS initially with Portainer CE managing Docker stacks.
   - Cloudflare Tunnel (`cloudflared`) routing HTTPS traffic without open inbound host firewall ports.
   - Separate databases, environment files (`.env`), credentials, and persistent Docker volumes for every stack.
3. **Data & Security Policy:**
   - Synthetic data fixtures only.
   - Mandatory search engine disallowance (`X-Robots-Tag: noindex, nofollow`, `robots.txt`).
   - Dedicated demonstration user accounts per external stakeholder.

---

## Consequences

### Positive:
- Clean, professional unified URL structure for all Alamia demos.
- Complete operational isolation between product demos: any stack can be restarted, reset, or destroyed independently.
- Zero open VPS ports, leveraging existing Cloudflare infrastructure.

### Negative / Tradeoffs:
- Slightly higher host RAM footprint due to running dedicated database containers per stack (mitigated via Docker container memory limits).
- Requires maintaining separate `.env` and Docker Compose manifests per demo stack.
