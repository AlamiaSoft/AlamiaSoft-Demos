# AI Knowledge Base Master Index

Welcome to the canonical AI knowledge base for the **AlamiaSoft Demos Platform** (`demos.alamiasoft.com`). This repository houses the orchestration, configuration, stacks, and central portal for hosting public and client-facing demonstrations of Alamia software products.

## The AI Bootstrap Read Order
If you are an AI Agent entering a fresh conversation, you **must** read the following documents in order before inspecting the source code or executing commands:

1. `README.md` (You are here: Master Index)
2. `.ai/permanent/architecture/01-system-architecture.md` (System boundaries, stack isolation, network routing)
3. `.ai/indexes/repository.md` (Mapping architecture concepts to directory structure)
4. `.ai/permanent/standards/01-coding-standards.md` (Stack conventions, security, synthetic data rules)
5. `.ai/transient/sprint/00-current-state.md` (Active sprint goals, focus, immediate priorities)

---

## Directory Structure

### Permanent Knowledge (Lives for Years)
* **`permanent/architecture/`**: High-level design intent, tradeoffs, and runtime isolation models.
* **`permanent/adr/`**: Architecture Decision Records. Contains index and history of decisions.
* **`permanent/glossary/`**: Single source of truth for platform terminology.
* **`permanent/standards/`**: Container conventions, synthetic seeding rules, and deployment policies.

### Transient Knowledge (Lives for Days)
* **`transient/sprint/`**: The current sprint's focus and objectives.
* **`transient/handoffs/`**: Session handoffs summarizing the immediate delta between chats.
* **`transient/backlog/`**: Pending tasks and delivery backlog.
* **`transient/repository-health.md`**: Tracking architecture drift, documentation coverage, and stack status.

### History & Meta
* **`history/`**: Architecture timeline sprint-by-sprint.
* **`indexes/`**: 
  * `repository.md`: Maps high-level concepts to source code directories and ADRs.
  * `dependency-map.md`: Mermaid graph of edge routing, tunnel ingress, and Docker containers.
* **`lessons/`**: Operational knowledge, debugging outcomes, and hosting post-mortems.
