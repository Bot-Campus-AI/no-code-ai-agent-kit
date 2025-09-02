
---

# Deploying n8n Workflows: from “demo” to “production”

## Quick decision tree

* **Demo today** → **A. Local + Tunnel (ngrok)**
* **Zero-ops hosted** → **B. n8n Cloud**
* **Cheap, controllable prod** → **C. Single VPS (Docker Compose)**
* **Scale / HA** → **D. Kubernetes**
* **Click-to-deploy PaaS** → **E. Render / Fly.io / Railway**
* **Only need a public webhook** → **F. Serverless Webhook (Cloud Run/Functions/Lambda) + private n8n**

---

# A) Local + Tunnel (fast demo)

**When:** workshops, quick proofs, testing Slack/WhatsApp webhooks.

### Steps

1. **Start n8n locally**

```bash
# Option 1: Node (mac/linux)
npx n8n

# Option 2: Docker
docker run -it --rm -p 5678:5678 n8nio/n8n:latest
```

2. **Expose port with ngrok**

```bash
# install (macOS with Homebrew)
brew install ngrok/ngrok/ngrok

# auth once (paste your token from dashboard.ngrok.com)
ngrok config add-authtoken YOUR_AUTH_TOKEN

# run tunnel
ngrok http 5678
```

Copy the **https** URL (e.g., `https://abc123.ngrok-free.app`).

3. **Use the Production webhook URL**

* In your Webhook node, copy the **Production** path: `/webhook/<id>`
* In Slack (or any integration), set **Request URL** to:

```
https://abc123.ngrok-free.app/webhook/<id>
```

4. **Prevent slash-command timeouts**

* Webhook node → **Response Mode: On Received**
* Response JSON:

```json
{ "response_type":"ephemeral","text":"Got it! Working on it…" }
```

**Pros:** 2–5 min setup
**Cons:** Tunnel URL may change; not for production

---

# B) n8n Cloud (managed)

**When:** you want SSL, domain, storage, and updates handled for you.

### Steps

1. Sign up: **n8n Cloud** → create workspace
2. **Import** workflows via UI or API
3. Set **Credentials** and **Environment Variables** (Secrets Manager)
4. **Activate** workflows → update Slack/Sheets/etc to use the new cloud **webhook base URL**

**Pros:** zero ops, SLA
**Cons:** monthly cost, less infra control

---

# C) Single VPS (Docker Compose) — sweet spot for most

**When:** stable production on one small server at low cost.

### Files to create (on your VPS)

1. **`.env`**

```dotenv
DOMAIN=your-domain.com
N8N_ENCRYPTION_KEY=generate_a_long_random_string
# optional: set timezone, etc.
GENERIC_TIMEZONE=Asia/Kolkata
```

2. **`docker-compose.yml`**

```yaml
version: "3.8"
services:
  n8n:
    image: n8nio/n8n:latest
    restart: unless-stopped
    environment:
      - N8N_ENCRYPTION_KEY=${N8N_ENCRYPTION_KEY}
      - N8N_SECURE_COOKIE=true
      - N8N_PROTOCOL=https
      - WEBHOOK_URL=https://${DOMAIN}
      - N8N_DIAGNOSTICS_ENABLED=false
      - N8N_RUNNERS_ENABLED=true
      - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
volumes:
  n8n_data:
```

3. **Reverse proxy & TLS (choose one)**

**Caddy (auto-SSL, simplest)** — `Caddyfile`

```caddy
${DOMAIN} {
  reverse_proxy 127.0.0.1:5678
}
```

Run:

```bash
# Ubuntu
sudo apt-get update && sudo apt-get install -y caddy docker.io docker-compose-plugin
sudo setcap cap_net_bind_service=+ep /usr/bin/caddy
docker compose up -d
sudo systemctl enable --now caddy
```

**Nginx + Certbot** (alternative)

```bash
sudo apt-get update && sudo apt-get install -y nginx docker.io docker-compose-plugin
sudo docker compose up -d
sudo apt-get install -y certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

Nginx site (proxy to :5678):

```nginx
server {
  listen 80;
  server_name your-domain.com;
  location / {
    proxy_pass http://127.0.0.1:5678;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $remote_addr;
  }
}
```

### Verify

```bash
curl -I https://your-domain.com
```

Update Slack/WhatsApp/other integrations to use:

```
https://your-domain.com/webhook/<id>
```

**Pros:** cheap, persistent, custom domain & SSL
**Cons:** you manage updates/backups (take weekly volume snapshots)

---

# D) Kubernetes (HA & scale)

**When:** multiple workflows, high availability, horizontal scaling.

### Minimal manifests (snippets)

**Deployment**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: { name: n8n }
spec:
  replicas: 2
  selector: { matchLabels: { app: n8n } }
  template:
    metadata: { labels: { app: n8n } }
    spec:
      containers:
        - name: n8n
          image: n8nio/n8n:latest
          env:
            - name: WEBHOOK_URL
              value: "https://n8n.your-domain.com"
            - name: N8N_ENCRYPTION_KEY
              valueFrom: { secretKeyRef: { name: n8n, key: encryptionKey } }
            - name: N8N_RUNNERS_ENABLED
              value: "true"
          ports: [{ containerPort: 5678 }]
          volumeMounts:
            - name: data
              mountPath: /home/node/.n8n
      volumes:
        - name: data
          persistentVolumeClaim: { claimName: n8n-pvc }
```

**PVC**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata: { name: n8n-pvc }
spec:
  accessModes: [ReadWriteOnce]
  resources: { requests: { storage: 10Gi } }
```

**Ingress (TLS)**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata: { name: n8n, annotations: { kubernetes.io/ingress.class: nginx, cert-manager.io/cluster-issuer: letsencrypt } }
spec:
  tls:
    - hosts: [ "n8n.your-domain.com" ]
      secretName: n8n-tls
  rules:
    - host: n8n.your-domain.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend: { service: { name: n8n, port: { number: 5678 } } }
```

**Pros:** resilience, rolling updates, autoscaling
**Cons:** highest complexity

---

# E) PaaS (Render / Fly.io / Railway)

**When:** you want Git-driven deploys and managed SSL without K8s.

### Generic steps

**Render**

1. New **Web Service** → Image: `n8nio/n8n:latest`
2. Add persistent disk: `/home/node/.n8n`
3. Env vars: `WEBHOOK_URL`, `N8N_ENCRYPTION_KEY`, `N8N_RUNNERS_ENABLED=true`
4. Map custom domain → Render manages SSL

**Fly.io**

```bash
flyctl launch --image n8nio/n8n:latest
flyctl volumes create n8n_data --size 10
# edit fly.toml: mount /home/node/.n8n
flyctl deploy
```

**Railway**

* Create service from Docker image
* Add volume and env vars
* Attach domain → enable SSL

**Pros:** simple pipeline, often free/low tiers
**Cons:** cold starts on free tiers; provider limits

---

# F) “Edge” Serverless Webhook + private n8n

**When:** you just need a public HTTPS endpoint but prefer to keep n8n private (VPN/IP allowlist).

### Pattern

* Cloud Run / Cloud Functions / Lambda exposes HTTPS
* Function validates request (signatures), then:

  * **Forwards** to your private n8n via VPN / allowlist, or
  * **Queues** message (Pub/Sub, SQS) that n8n consumes on a private pull

**Example: Google Cloud Run (Node)**

```bash
# Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
CMD ["node","server.js"]
```

```js
// server.js
import express from "express";
import fetch from "node-fetch";
const app = express(); app.use(express.json());

app.post("/webhook", async (req,res) => {
  // TODO: verify Slack signature, etc.
  await fetch(process.env.N8N_PRIVATE_URL + "/webhook/<id>", {
    method:"POST",
    headers: { "Content-Type":"application/json" },
    body: JSON.stringify(req.body)
  });
  res.status(200).json({ ok: true });
});

app.listen(process.env.PORT || 8080);
```

Deploy with:

```bash
gcloud run deploy n8n-edge --source . --region=YOUR-REGION --allow-unauthenticated
```

**Pros:** very resilient ingress, cheap
**Cons:** two moving parts

---

## CI/CD: move workflows between envs

**Export → Git → Import via REST**

```bash
# create
curl -X POST https://n8n.your-domain.com/rest/workflows \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d @workflow.json

# update
curl -X PATCH https://n8n.your-domain.com/rest/workflows/WORKFLOW_ID \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d @workflow.json

# activate
curl -X POST https://n8n.your-domain.com/rest/workflows/WORKFLOW_ID/activate \
  -H "Authorization: Bearer YOUR_API_KEY"
```

**Repo layout**

```
/workflows/
  lead/lead_slack_intake.json
  support/faq_agent.json
/env/
  .env.example
infra/
  docker-compose.yml
  Caddyfile
```

---

## Secrets & config (do this everywhere)

* Set **`N8N_ENCRYPTION_KEY`** (store safely; don’t rotate casually)
* Put API keys in **Environment Variables** or **Credentials**, never in nodes
* **`WEBHOOK_URL`** = your public https base (fixes callback links)
* **`N8N_RUNNERS_ENABLED=true`** for reliability on heavy/long jobs
* Optional: **`N8N_DIAGNOSTICS_ENABLED=false`** to disable telemetry

---

## Production hardening checklist

* HTTPS + HSTS
* Verify webhook signatures (Slack/Stripe) or IP allowlist
* Backups: volume snapshots + workflow JSON exports
* Alerts: `Error Trigger` node → Slack/Email on failures
* Rate limits on proxy; Health checks & uptime monitoring
* Version your workflows in Git; tag releases

---

## Rollout playbook (5 steps)

1. **Develop** locally; test with Postman
2. **Expose** with ngrok for real service callbacks
3. **Commit** JSON + `.env.example` to Git
4. **Deploy** to VPS/PaaS/K8s (pick one pattern above)
5. **Switch** integrations (Slack, WhatsApp API, etc.) to the new `WEBHOOK_URL`; monitor

---
