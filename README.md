# NexusCare-Sim

Distributed multi-tenant event-driven autonomous **hospital logistics** simulation (CS capstone).

NexusCare-Sim simulates a ward logistics rover: local video + telemetry, on-device staff/patient face matching, async Vision-LLM patient-safety analysis, and a real-time nursing command center.

## Architecture (high level)

```
Python CareBot Sim ──frames──► InsightFace ──► Cosine Matcher
       │                                           │
       │ ward telemetry                            ├── on-duty staff/patient → log OK
       ▼                                           └── unrecognized/unattended → Redis/Celery → Vision-LLM
FastAPI Gateway ◄──────────────────────────────────────────────────────────────┘
       │ WebSocket
       ▼
Next.js Ward Command Center

Databases: PostgreSQL (hospitals/staff/patients) · Qdrant (face + scene vectors) · ClickHouse (patrol telemetry)
```

## Monorepo layout

| Path | Role |
|------|------|
| `simulator/` | OpenCV ward video loop + mock CareBot telemetry (Python) |
| `backend/` | FastAPI multi-tenant gateway + WebSockets |
| `workers/` | Celery workers (Vision-LLM patient-safety jobs) |
| `frontend/` | Next.js ward command center |
| `infra/` | Docker Compose and shared infra configs |

## Stack

- **Edge sim:** Python, OpenCV, InsightFace / ArcFace
- **API:** FastAPI, WebSockets, multi-tenant RLS (`org_id` = hospital network)
- **Async:** Redis + Celery
- **AI:** Vision-LLM (structured patient-safety JSON)
- **Data:** PostgreSQL, Qdrant, ClickHouse
- **UI:** Next.js

## Domain alerts (examples)

- Unrecognized visitor in a restricted wing
- Fall-risk / unattended patient out of assigned zone
- Hazardous spill or blocked emergency pathway

Vision-LLM response fields: `is_patient_hazard`, `severity_rating` (`LOW` | `MEDIUM` | `CRITICAL`), `incident_summary`, `recommended_staff_action`.

## Status

Scaffolding in progress. Same distributed stack as planned; domain pivoted to hospital logistics / patient safety.
