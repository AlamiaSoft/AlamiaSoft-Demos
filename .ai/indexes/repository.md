# Repository Code & Architecture Map

This index maps architectural concepts, ADRs, and deployment targets to concrete directories and files within this repository.

---

## Directory Mapping

| Directory / File | Architectural Role | ADR / Spec Reference |
| :--- | :--- | :--- |
| [`.agents/`](file:///e:/Alamia/AlamiaSoft-Demos/.agents) | AI Pair Programming rules & registered lifecycle skills | `AGENTS.md` |
| [`.ai/`](file:///e:/Alamia/AlamiaSoft-Demos/.ai) | Master AI Knowledge Base (permanent architecture, standards, sprint state, handoffs) | [Master Index](file:///e:/Alamia/AlamiaSoft-Demos/.ai/README.md) |
| [`.alamia/`](file:///e:/Alamia/AlamiaSoft-Demos/.alamia) | State configuration discovered and reconciled by Alamia Skills System | `project.json` |
| [`Alamia-Skills-System/`](file:///e:/Alamia/AlamiaSoft-Demos/Alamia-Skills-System) | Git submodule providing universal lifecycle orchestration layer | `README.md` |
| [`docs/`](file:///e:/Alamia/AlamiaSoft-Demos/docs) | Project inception memos and decision notes | [`00-start.md`](file:///e:/Alamia/AlamiaSoft-Demos/docs/00-start.md) |
| [`demos.alamiasoft.com/`](file:///e:/Alamia/AlamiaSoft-Demos/demos.alamiasoft.com) | Central Demo Portal web application source code & stack configuration | [ADR-0001](file:///e:/Alamia/AlamiaSoft-Demos/.ai/permanent/adr/0001-central-demos-subdomain-and-isolated-docker-stacks.md) |
| [`ingress/`](file:///e:/Alamia/AlamiaSoft-Demos/ingress) | Cloudflare Tunnel configuration & ingress host routing documentation | [`cloudflare-tunnel.md`](file:///e:/Alamia/AlamiaSoft-Demos/ingress/cloudflare-tunnel.md) |
| `stacks/accounts/` | Alamia Accounts Demo Docker Compose stack (source: `https://github.com/AlamiaSoft/AlamiaAccounts`) | [System Architecture](file:///e:/Alamia/AlamiaSoft-Demos/.ai/permanent/architecture/01-system-architecture.md) |
| `stacks/travelos/` | TravelOS Demo Docker Compose stack (future phase) | [System Architecture](file:///e:/Alamia/AlamiaSoft-Demos/.ai/permanent/architecture/01-system-architecture.md) |
| `stacks/connect/` | AlamiaConnect CRM Demo Docker Compose stack (future phase) | [System Architecture](file:///e:/Alamia/AlamiaSoft-Demos/.ai/permanent/architecture/01-system-architecture.md) |

---

## ADR Traceability

- **[ADR-0001](file:///e:/Alamia/AlamiaSoft-Demos/.ai/permanent/adr/0001-central-demos-subdomain-and-isolated-docker-stacks.md)**: Establishes `demos.alamiasoft.com` as central namespace, single VPS with Portainer, Cloudflare Tunnel ingress, and isolated stacks for Accounts, TravelOS, and Connect. Implemented via `demos.alamiasoft.com/` and `stacks/`.
