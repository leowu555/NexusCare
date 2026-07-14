# AegisBot-Sim

Distributed multi-tenant event-driven robotic simulation platform (CS capstone).

AegisBot-Sim simulates an autonomous security robot: local video + telemetry, on-device face embedding, async Vision-LLM incident analysis, and a real-time operations dashboard.

## Architecture (high level)

```
Python Simulator ──frames──► InsightFace ──► Cosine Matcher
       │                                         │
       │ telemetry                               ├── known  → check-in
       ▼                                         └── unknown → Redis/Celery → Vision-LLM
FastAPI Gateway ◄──────────────────────────────────────────────┘
       │ WebSocket
       ▼
Next.js Dashboard

Databases: PostgreSQL (tenants/users) · Qdrant (vectors) · ClickHouse (telemetry)
```

## Monorepo layout

| Path | Role |
|------|------|
| `simulator/` | OpenCV video loop + mock telemetry (Python) |
| `backend/` | FastAPI multi-tenant gateway + WebSockets |
| `workers/` | Celery workers (Vision-LLM jobs) |
| `frontend/` | Next.js operations dashboard |
| `infra/` | Docker Compose and shared infra configs |

## Stack

- **Edge sim:** Python, OpenCV, InsightFace / ArcFace
- **API:** FastAPI, WebSockets, multi-tenant RLS (`org_id`)
- **Async:** Redis + Celery
- **AI:** Vision-LLM (structured JSON incidents)
- **Data:** PostgreSQL, Qdrant, ClickHouse
- **UI:** Next.js

## Status

Scaffolding in progress. Components will be added incrementally.
