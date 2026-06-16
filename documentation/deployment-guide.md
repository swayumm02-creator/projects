# Deployment Guide — Aegis Health

1. Click **Deploy Now**
2. From "Deployment settings" you can:
   - Set/rotate env vars (swap `STRIPE_API_KEY` to `sk_live_...` for real payments)
   - Add a custom domain
   - Roll back to any previous deploy



## Self-hosted (Docker)

A reference `docker-compose.yml`:

```yaml
version: "3.9"
services:
  mongo:
    image: mongo:7
    volumes: ["mongo-data:/data/db"]
  backend:
    build: ./backend
    environment:
      MONGO_URL: mongodb://mongo:27017
      DB_NAME: aegis
      
      JWT_SECRET: ${JWT_SECRET}
      STRIPE_API_KEY: ${STRIPE_API_KEY}
    ports: ["8001:8001"]
    depends_on: [mongo]
  frontend:
    build: ./frontend
    environment:
      REACT_APP_BACKEND_URL: http://backend:8001
    ports: ["3000:3000"]
volumes:
  mongo-data:
```

## Post-deploy checklist
- [ ] Swap Stripe to live keys
- [ ] Narrow CORS to your domain (currently `*`)
- [ ] Set a strong `JWT_SECRET` (rotate quarterly)
- [ ] Configure custom domain + email DKIM if using Resend
- [ ] Add Stripe webhook in dashboard pointing to `/api/webhook/stripe`
- [ ] Set up MongoDB backups (daily snapshot)
- [ ] Enable error tracking (Sentry)

## Health checks
- Backend: `GET /api/` → `{"app":"Aegis Health","status":"ok"}`
- Frontend: `GET /` → landing page renders
