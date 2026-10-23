# Domain & Platform Glossary

This glossary establishes canonical terminology for the AlamiaSoft Demos Platform.

---

### Central Demo Portal
The primary landing application hosted at `demos.alamiasoft.com`. Serves as the public catalog and navigation directory to all available Alamia product demonstrations, providing product summaries, status badges, and evaluation guidelines.

### Product Demo Stack
An independently deployable Docker Compose application bundle representing a single Alamia product (e.g. `accounts.demos.alamiasoft.com`). Contains the product runtime, dedicated database, isolated network, and persistent storage volumes.

### Ingress Network (`cloudflare-tunnel-net`)
A shared internal Docker network allowing the `cloudflared` tunnel container to route incoming HTTP requests directly to each product stack's web service without binding host ports.

### Blast Radius Containment
The architectural design principle ensuring that failures, resource leaks, database wipes, or software crashes in one demo stack have zero impact on adjacent demo stacks or the host server.

### Synthetic Fixture / Seed
Procedurally generated, fictitious data used exclusively to populate demonstration databases. Designed to showcase rich features (e.g. multi-currency journal entries, profit & loss reports, sales pipelines) without touching real enterprise data.

### Demonstration Persona
A pre-configured user profile created for evaluators or clients (e.g. Mr. Muzaffar) equipped with tailored role permissions, bookmarked dashboards, and walkthrough scenarios.
