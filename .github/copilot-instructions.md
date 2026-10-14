# Copilot instructions for this repository

## Project overview

This repository is a small FastAPI app for managing extracurricular activities at Mergington High School. The implementation is intentionally compact and centered around a single application module instead of a larger layered architecture.

## Build, test, and validation commands

- Install dependencies:
  - `python -m pip install -r requirements.txt`
- Run the API locally:
  - `python -m uvicorn src.app:app --reload`
- Smoke-check the app import:
  - `python -c "from src.app import app; print(app.title)"`
- Run the project test suite:
  - `pytest`
- Run a single test file or case if tests are added later:
  - `pytest -q tests/test_app.py`
  - `pytest -q tests/test_app.py -k signup`

Notes:
- `pytest.ini` sets `pythonpath = .`, so imports should resolve cleanly from the repository root.
- There is no dedicated lint, formatting, or build script in this repo right now; `pytest` is the primary verification command.

## High-level architecture

- `src/app.py` is the heart of the app. It creates the FastAPI app, defines the routes, and stores the in-memory activity database.
- `src/static/` contains the browser UI served by FastAPI. The root route redirects to `/static/index.html`.
- `README.md` is the canonical source for the public API contract and the in-memory data model.
- The application keeps all state in memory (`activities` dictionary) and resets when the process restarts; there is no database, migration system, or persistent storage layer.
- Route paths use the activity name as the identifier, for example `/activities/{activity_name}/signup` and `/activities/{activity_name}/participants`.

## Key conventions in this codebase

- Keep this project lightweight: prefer updating `src/app.py` and the static assets over creating a large service layer or package structure.
- Activity names are dictionary keys and route path segments; student emails are the participant identifiers.
- The app validates business rules directly in endpoint functions instead of via a separate validation layer.
- Duplicate signups are rejected with HTTP 400; missing participants on delete are rejected with HTTP 404; nonexistent activities are rejected with HTTP 404.
- When a feature changes the API surface, keep the README, the static UI, and the endpoint behavior aligned.
- The repo is intentionally stateful only in memory; avoid introducing persistence unless the task specifically requires it.

## Working style for future changes

- Prefer small, focused edits around the existing API in `src/app.py`.
- If you add tests, use `pytest` and keep them close to the app behavior rather than introducing unrelated test infrastructure.
- Preserve the current route layout and response structure unless the task explicitly requires a contract change.
