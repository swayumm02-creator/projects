# Setup Guide — Aegis Health

## Prerequisites
- Python 3.11+ · Node 18+ · Yarn · MongoDB 6+

## 1. Clone & install

```bash
git clone <repo>
cd aegis-health
```

## 2. Backend

```bash
cd backend
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
```

Create `backend/.env`:
```
MONGO_URL="mongodb://localhost:27017"
DB_NAME="aegis_dev"
CORS_ORIGINS="*"
JWT_SECRET="change-me-in-prod"
```

Run:
```bash
uvicorn server:app --host 0.0.0.0 --port 8001 --reload
```

## 3. Frontend

```bash
cd frontend
yarn install
```

Create `frontend/.env`:
```
REACT_APP_BACKEND_URL=http://localhost:8001
```

Run:
```bash
yarn start    # → http://localhost:3000
```

## 4. Create the four roles

Open `/auth/signup` and create one user per role:
- `patient@demo.com` (Patient)
- `doctor@demo.com` (Doctor)
- `admin@demo.com` (Admin)
- `research@demo.com` (Researcher)

Each role redirects to its own dashboard.

## 5. Tests

```bash
cd backend && pytest tests/
```
