# SmartDash CS

SmartDash CS is a SaaS MVP that turns customer service, order, returns, refunds, SLA, agent performance, and operational spreadsheets into interactive dashboards.

The core architecture keeps trust boundaries clean:

- Gemini/LLM understands metadata, recommends dashboard logic, and writes explanations.
- FastAPI/Pandas calculates every KPI, chart value, chat answer, and data quality score.
- Next.js visualizes the dashboard and keeps the workflow simple for non-technical team leaders.

## What Is Built

- Next.js + TypeScript frontend with Tailwind, shadcn-style local components, Recharts, TanStack Table, Framer Motion-ready structure, and lucide icons.
- FastAPI backend with CSV/XLSX parsing, multi-sheet Excel handling, profile generation, PII detection/redaction, AI-safe payloads, Gemini provider abstraction, rule-based fallback planning, KPI/chart generation, saved dashboard JSON, and chat-with-data deterministic query intents.
- Supabase/Postgres-ready schema in `db/schema.sql`.
- Product design docs in `docs/`.
- Demo dashboard for immediate UI exploration when the backend is not running.

## Local Setup

1. Install frontend dependencies:

```powershell
npm.cmd install
```

2. Install backend dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r backend\requirements.txt
```

3. Create `.env` from `.env.example`:

```powershell
Copy-Item .env.example .env
```

4. Optional: add `GEMINI_API_KEY` to `.env`. Without it, SmartDash CS still generates dashboards using deterministic rule-based planning.

## Run Locally

Start the backend:

```powershell
.\.venv\Scripts\python.exe -m uvicorn backend.app.main:app --reload --port 8000
```

Start the frontend:

```powershell
npm.cmd run dev
```

Open `http://localhost:3000`.

## Key Pages

- `/dashboard` - product home with demo dashboard.
- `/upload` - upload Excel/CSV, profile data, and generate dashboard.
- `/datasets` - dataset library.
- `/dashboards` - dashboard library.
- `/insights` - AI insights view.
- `/settings` - workspace, privacy, and API-key settings surface.

## API Boundary

Important endpoints are implemented under the FastAPI service:

- `POST /api/upload`
- `POST /api/datasets/{id}/profile`
- `POST /api/datasets/{id}/analyze`
- `POST /api/datasets/{id}/generate-dashboard`
- `GET /api/datasets/{id}/preview`
- `GET /api/datasets/{id}/quality-report`
- `GET /api/dashboards`
- `POST /api/chat-with-data`

## Verification

The current implementation has been checked with:

```powershell
npm.cmd run typecheck
npm.cmd run build
```

The backend service layer was smoke-tested with Pandas profiling, rule-based planning, and dashboard generation.

## Production Next Steps

- Replace local demo auth with Supabase Auth.
- Store parsed datasets and raw files in Supabase Storage or S3.
- Persist all metadata through Postgres tables and enable RLS policies.
- Add background jobs for large files and OCR processing.
- Add public/private Google Sheets import.
- Add Playwright-backed PDF/PNG export.
- Add automated tests for upload parsing, semantic detection, KPI calculations, and chat query intents.

