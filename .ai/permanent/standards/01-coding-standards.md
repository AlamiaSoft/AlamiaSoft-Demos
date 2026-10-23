# Platform & Stack Engineering Standards

> **Scope:** Repository structure, Docker Compose standards, configuration hygiene, and synthetic seeding.

---

## 1. Repository Structure & Naming Conventions

The repository organizes demo stacks under dedicated top-level directories:

```text
AlamiaSoft-Demos/
├── .agents/                      # AI pair-programming instructions and lifecycle skills
├── .ai/                          # AI Knowledge Base (permanent, transient, indexes)
├── .alamia/                      # Alamia Skills System project profile
├── Alamia-Skills-System/         # Core orchestration submodule
├── docs/                         # Human-authored decision memos and specifications
├── demos.alamiasoft.com/         # Central Portal web application stack
└── stacks/                       # Product demo stack definitions
    ├── accounts/                 # Alamia Accounts demo stack (docker-compose, .env.example, scripts)
    ├── travelos/                 # TravelOS demo stack (future)
    └── connect/                  # AlamiaConnect CRM demo stack (future)
```

### Stack Naming Convention
* Compose project name / stack name: `alamia-demo-<product>` (e.g., `alamia-demo-portal`, `alamia-demo-accounts`).
* Container name format: `<stack_name>-<service>` (e.g., `alamia-demo-accounts-app`, `alamia-demo-accounts-db`).
* Volume name format: `<stack_name>-<service>-data` (e.g., `alamia-demo-accounts-db-data`).

---

## 2. Docker Compose Standards

Every product demo stack must provide a standalone `docker-compose.yml` meeting these requirements:

1. **Self-Contained Persistence:**
   - Must define dedicated named volumes for databases and uploads.
   - Must NOT bind-mount system directories from the host outside the stack folder.
2. **Resource Limits:**
   - Every service container must define explicit CPU and memory reservations/limits to prevent runaway processes:
     ```yaml
     deploy:
       resources:
         limits:
           cpus: '1.0'
           memory: 768M
         reservations:
           memory: 256M
     ```
3. **Restart Policy:**
   - Use `restart: unless-stopped` across all service definitions.
4. **Healthchecks:**
   - Define Docker native `healthcheck` on both the application container and database container to support orchestration readiness checks.
5. **Port Exposure:**
   - Do **not** bind database ports (e.g. `5432:5432` or `3306:3306`) to the host network interface. Keep databases strictly internal to the stack network.
   - Web containers expose either a loopback port on `127.0.0.1:<port>` or attach to a shared internal Docker network (`cloudflare-tunnel-net`) for Cloudflared routing.

---

## 3. Configuration & Secrets Hygiene

1. **`.env.example` Mandatory:**
   - Every stack directory must maintain a fully commented `.env.example` documenting all configuration keys, sensible default ports, and dummy secret values.
2. **Never Commit `.env`:**
   - Root `.gitignore` must forbid `.env`, `.env.local`, `.env.*.local`, and any file containing sensitive credentials.
3. **Decoupled Secrets:**
   - Never reuse `APP_KEY`, database passwords, or auth salts between different demo stacks.

---

## 4. Synthetic Data & Seeding Rules

1. **Zero Real-World Data:**
   - No customer databases, real tax numbers, client legal names, or real banking transaction logs may enter this repository or demo containers.
2. **Deterministic Synthetic Seeders:**
   - Demos must supply an automated seed script (e.g. `scripts/seed-demo.sh` or framework seeder commands).
   - Test data should feature recognizable, realistic fictitious entities (e.g., "Apex Logistics Ltd", "Beacon Consulting FZE") with rich transaction history, pending invoices, and balanced ledgers.
3. **Dedicated Demonstration Persona:**
   - Every demo must provide pre-seeded credentials for client evaluation (e.g., `demo.muzaffar@alamiasoft.com` with a known, secure initial password).

---

## 5. Search Engine Anti-Indexing Standard

All web servers (Nginx/Apache/Caddy/Node) deployed within any demo stack must enforce anti-crawling directives:

1. **HTTP Headers:**
   ```nginx
   add_header X-Robots-Tag "noindex, nofollow, noarchive, nosnippet" always;
   ```
2. **Static `robots.txt`:**
   Served from root:
   ```txt
   User-agent: *
   Disallow: /
   ```
