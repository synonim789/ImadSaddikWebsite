# AGENTS.md

This repository contains a Vue 3 frontend and a FastAPI backend for Imad Saddik's personal website. The search layer uses Meilisearch, while visitor tracking stores data in a local SQLite database.

## Repository map

- Frontend app: [frontend](frontend)
- Backend API: [backend](backend)
- App startup and local setup: [README.md](README.md)
- Seed data and Meilisearch notes: [backend/seed/README.md](backend/seed/README.md)
- Infrastructure scripts and deployment helpers: [infrastructure](infrastructure)

## Working conventions

- Prefer small, focused changes that match the existing structure and naming patterns.
- Keep frontend logic in the Vue app under [frontend/src](frontend/src), not in the root.
- Keep backend routes, services, and models grouped under [backend/api](backend/api), [backend/services](backend/services), [backend/models](backend/models), and [backend/core](backend/core).
- Treat Meilisearch as a dependency for search and article indexing; do not replace it with ad hoc storage unless the task specifically requires it.
- Visitor tracking is SQLite-backed via [backend/database.py](backend/database.py); do not bypass the existing DB helper functions when adding new database writes.

## Local development commands

### Frontend

```bash
cd frontend
pnpm install
pnpm dev
```

Useful checks:

```bash
cd frontend
pnpm lint
pnpm test:run
pnpm test:e2e
```

### Backend

```bash
cd backend
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements-dev.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Useful checks:

```bash
cd backend
pytest
```

## Environment and runtime expectations

- The backend reads configuration from the environment and defaults to a local Meilisearch instance at `http://localhost:7700`.
- For local development, create the backend `.env` file by following the setup in [README.md](README.md).
- Seeding/search data is handled by [backend/scripts/seed_meilisearch.py](backend/scripts/seed_meilisearch.py) and the files in [backend/seed](backend/seed).
- The backend app health endpoint is exposed at `/api/health` in [backend/main.py](backend/main.py).

## Testing guidance

- Backend tests live under [backend/tests](backend/tests) and are organized by unit and integration coverage.
- Frontend tests use Vitest and Playwright as defined in [frontend/package.json](frontend/package.json).
- When fixing a bug, add or update the most relevant test in the closest existing test file before finalizing the patch.

## Prefer these docs for details

- [README.md](README.md) for setup and deployment context
- [backend/seed/README.md](backend/seed/README.md) for Meilisearch indexing data
- [CONTRIBUTING.md](CONTRIBUTING.md) for contribution expectations
- [backend/requirements-dev.txt](backend/requirements-dev.txt) for the Python dev toolchain

## Do not duplicate

Avoid re-documenting the same setup instructions already present in [README.md](README.md). Link to those docs instead and keep this file focused on project-specific behavior, boundaries, and commands that help AI agents be productive quickly.
