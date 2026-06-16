# API Reference — Aegis Health

Base URL (dev): `http://localhost:8001/api`

All authenticated endpoints require header: `Authorization: Bearer <jwt>`

---

## Auth

### `POST /auth/signup`
```json
{ "email": "u@x.com", "password": "p≥6chars", "name": "Jane",
  "role": "patient|doctor|admin|researcher", "age": 30, "gender": "female" }
```
→ `{ "token": "...", "user": {...} }`

### `POST /auth/login`
```json
{ "email": "u@x.com", "password": "..." }
```

### `GET /auth/me`
Returns current user.

---

## Health

### `POST /health/record`
```json
{ "age":38, "gender":"male", "height_cm":175, "weight_kg":82,
  "systolic":128, "diastolic":84, "glucose":102, "cholesterol":210, "hdl":48,
  "smoker":false, "alcohol":false, "exercise_min_week":90,
  "family_history":["diabetes"] }
```
→ Full intelligence: `{ scores, predictions[5], wearable[30], anomalies, alerts, recommendations }`

### `GET /health/intelligence`
Latest intelligence or `{ has_record: false }`.

### `GET /wearable/series?days=30`
### `GET /wearable/live`

---

## NLP + Assistant

### `POST /nlp/symptoms`
```json
{ "text": "Chest pain when climbing stairs, shortness of breath for 3 days" }
```
→ `{ symptoms[], entities[], possible_conditions[], red_flags[], triage }`

### `POST /assistant/chat` (non-streaming)
### `POST /assistant/stream` (SSE)
Body: `{ "message": "...", "session_id": "optional" }`
SSE format: `data: <token>\n\n` until `data: [DONE]\n\n`

**Free tier**: limited to 10 chats. Premium = unlimited.

### `GET /assistant/history`
### `POST /ocr/report` (multipart, field=file)

---

## Tools

### `POST /twin/simulate`
```json
{ "weight_change_kg": -5, "exercise_change_min": 60,
  "smoking_quit": true, "sleep_hours_target": 7.5 }
```

### `GET /graph/knowledge`
### `GET /reports/health.pdf` → `application/pdf`

---

## Role-protected

| Endpoint | Allowed roles |
|---|---|
| `GET /doctor/patients` | doctor, admin |
| `GET /admin/population` | admin, researcher |
| `GET /research/models` | researcher, admin |

---

## Billing

### `GET /billing/me`
```json
{ "is_premium": false, "packages": [...], "usage": {"chats_used": 3, "chat_limit_free": 10} }
```

### `POST /payments/checkout`
```json
{ "package_id": "premium_monthly", "origin_url": "https://yoursite.com" }
```
→ `{ "url": "https://checkout.stripe.com/...", "session_id": "cs_test_..." }`

### `GET /payments/status/{session_id}`
Idempotent. Poll up to 5× × 2s from the success page.

### `POST /webhook/stripe`
Stripe webhook target. Configure in Stripe dashboard pointing here.

---

## Error codes
- `401` — missing/invalid JWT
- `402` — free tier limit reached (chat endpoints)
- `403` — wrong role
- `404` — resource not found
- `409` — email already exists (signup)
- `422` — payload validation failed
