Quick decision: Use `demos.alamiasoft.com` as the central demo platform. I would reserve `demos.alamiaconnect.com` for AlamiaConnect-specific demos if needed later.

Since Alamia Accounts, TravelOS, and future products belong to the broader Alamia software ecosystem, a shared demo domain under AlamiaSoft gives us a cleaner product-neutral structure.

## Proposed domain structure

## demos.alamiasoft.com

Central demo portal · shared infrastructure

Recommended

accounts.demos.alamiasoft.com

Alamia Accounts — first demo for Mr. Muzaffar

travelos.demos.alamiasoft.com

TravelOS — when its demo is ready

connect.demos.alamiasoft.com

AlamiaConnect CRM — optional future demo

## Deployment rules

- One shared VPS initially, using Docker and Portainer, with each application deployed as a separate stack.
- Cloudflare Tunnel for HTTPS and routing, consistent with your existing infrastructure approach.
- Separate databases, credentials, volumes and environment variables for each demo. Never share application data across demos.
- Demo data only: synthetic companies, users and transactions; no real business financial data.
- Access controls: give Mr. Muzaffar a dedicated account. Keep the portal and demos out of search indexing, and use appropriate authentication or Cloudflare Access where practical.

One important distinction: a shared domain does not mean a shared application runtime or database. Keep each product independently deployable and removable.

Decision: Reserve `demos.alamiasoft.com` as the central demo namespace and deploy Alamia Accounts first at `accounts.demos.alamiasoft.com`. This is simple enough for today and scales to the wider Alamia portfolio without committing us to a separate demo server for every product.