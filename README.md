# ORCA Analytics

A marine analytics prototype for exploring ocean observations, source availability, and risk scenarios. The Next.js dashboard calls a FastAPI service that exposes query, marine-data, and risk APIs. The repository includes ingestion, provenance, and evaluation modules.

[Architecture](docs/architecture.md) · [API contract](docs/api-contract.md) · [Data dictionary](docs/data-dictionary.md)

## Repository layout

| Path | What to inspect |
| --- | --- |
| [apps/web](apps/web) | Next.js dashboard and map interface |
| [apps/api/app](apps/api/app) | FastAPI routes, ingestion, source adapters, and risk services |
| [packages/shared-types](packages/shared-types) | Types shared across the project |
| [evaluation](evaluation) | Benchmark runners, fixtures, and evaluation metrics |
| [tests](tests) | Automated test suite |

This is the analytics implementation. The separate [ORCA SeaSafety repository](https://github.com/JSR2406/ORCA-SeaSafety-SIh2026) contains the Smart India Hackathon prototype. Check source availability and provenance when interpreting outputs, and use official advisories for operational decisions.

## Run locally

The backend uses Python and PostgreSQL with PostGIS. Start from the [backend environment example](apps/api/.env.example) and use the [deployment guide](docs/phase10-deployment.md) for service configuration. Enable provider integrations only after adding development credentials for the sources you plan to query.

```bash
cd apps/api
python -m venv .venv
# Activate .venv before installing dependencies.
python -m pip install -r requirements.txt
cp .env.example .env
# Set DATABASE_URL and any provider settings needed for your local run.
alembic -c alembic.ini upgrade head
uvicorn app.main:app --reload --port 8000
```

In a second terminal, from the repository root:

```bash
cd apps/web
npm ci
npm run dev
```

Use the address printed by Next.js for the dashboard and `http://localhost:8000/docs` for the development API reference.

## Inspect the workflow

1. Use the dashboard or API to submit an ocean-data query.
2. Inspect the source information and query results alongside the generated explanation.
3. Review the [API contract](docs/api-contract.md) for request and response details and the [evaluation package](evaluation) for fixtures and scoring code.

Related references include the [architecture](docs/architecture.md), [data dictionary](docs/data-dictionary.md), and [safety notes](docs/safety.md).

The project integrates software and data from providers including the Argo Program, INCOIS, Sarvam AI, ElevenLabs, and OpenRouter.
