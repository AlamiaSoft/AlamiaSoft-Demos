# Central Demo Portal (`demos.alamiasoft.com`)

The primary landing portal and product demonstration catalog for AlamiaSoft software applications.

---

## Deployment Instructions

### Option 1: Quick Run with Docker Compose
```bash
cd demos.alamiasoft.com
cp .env.example .env
docker compose up -d --build
```
The portal will be accessible locally at `http://localhost:8080` (or `PORTAL_PORT`).

### Option 2: Deploy in Portainer CE
1. In Portainer, go to **Stacks** > **Add stack**.
2. Name the stack: `alamia-demo-portal`.
3. Paste the contents of `docker-compose.yml` (or select Repository deployment).
4. Set environment variable `PORTAL_PORT=8080`.
5. Click **Deploy the stack**.

---

## Cloudflare Tunnel Ingress Rule
In Cloudflare Zero Trust Dashboard > Networks > Tunnels > `alamiaconnect-vps-01` (`046f1c34-6e56-401a-b871-1a82238c4baf`):
- **Public Hostname:** `demos.alamiasoft.com`
- **Service:** `HTTP`
- **URL:** `localhost:8080` (or `127.0.0.1:8080`)
