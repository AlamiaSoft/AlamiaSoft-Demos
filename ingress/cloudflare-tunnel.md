# Cloudflare Tunnel Configuration & Ingress Guide

> **Active Tunnel:** `alamiaconnect-vps-01`  
> **Tunnel ID:** `046f1c34-6e56-401a-b871-1a82238c4baf`  
> **Daemon Type:** `cloudflared`  
> **Hosting VPS:** Shared VPS (Portainer CE + Docker)  

---

## 1. Cloudflare Dashboard Ingress Rules

To route public traffic to the internal Docker containers running on the VPS, configure the Public Hostname ingress rules under:
**Cloudflare One (Zero Trust) > Networks > Tunnels > `alamiaconnect-vps-01` > Public Hostnames**

| Public Hostname | Service Type | URL / Destination | Target Stack / Container |
| :--- | :--- | :--- | :--- |
| `demos.alamiasoft.com` | `HTTP` | `localhost:8080` | `alamia-demo-portal` (Landing catalog) |
| `accounts.demos.alamiasoft.com` | `HTTP` | `localhost:8081` | `alamia-demo-accounts-app` (Alamia Accounts Demo) |
| `travelos.demos.alamiasoft.com` *(Phase 2)* | `HTTP` | `localhost:8082` | `alamia-demo-travelos-app` (TravelOS Demo) |
| `connect.demos.alamiasoft.com` *(Phase 3)* | `HTTP` | `localhost:8083` | `alamia-demo-connect-app` (CRM Demo) |

---

## 2. Equivalent `config.yml` (For Host-Managed `cloudflared`)

If `cloudflared` is running via local configuration file (`/etc/cloudflared/config.yml` or Docker volume) rather than Remotely Managed through the Zero Trust dashboard:

```yaml
tunnel: 046f1c34-6e56-401a-b871-1a82238c4baf
credentials-file: /etc/cloudflared/046f1c34-6e56-401a-b871-1a82238c4baf.json

ingress:
  # 1. Central Demos Portal
  - hostname: demos.alamiasoft.com
    service: http://localhost:8080
    originRequest:
      noTLSVerify: true
      connectTimeout: 10s

  # 2. Alamia Accounts Demo (Target: Mr. Muzaffar)
  - hostname: accounts.demos.alamiasoft.com
    service: http://localhost:8081
    originRequest:
      noTLSVerify: true
      connectTimeout: 15s

  # 3. TravelOS Demo (Future)
  - hostname: travelos.demos.alamiasoft.com
    service: http://localhost:8082

  # 4. AlamiaConnect CRM Demo (Future)
  - hostname: connect.demos.alamiasoft.com
    service: http://localhost:8083

  # Catch-all 404 rule (Required by cloudflared)
  - service: http_status:404
```

---

## 3. Port Allocation Registry on Host

To avoid port collisions across independent stacks on the shared VPS:

| Port | Service | Subdomain | Compose Stack Location |
| :--- | :--- | :--- | :--- |
| **8080** | Central Demo Portal | `demos.alamiasoft.com` | `demos.alamiasoft.com/` |
| **8081** | Alamia Accounts Demo Web | `accounts.demos.alamiasoft.com` | `stacks/accounts/` |
| **8082** | TravelOS Demo Web | `travelos.demos.alamiasoft.com` | `stacks/travelos/` |
| **8083** | AlamiaConnect CRM Demo Web | `connect.demos.alamiasoft.com` | `stacks/connect/` |
