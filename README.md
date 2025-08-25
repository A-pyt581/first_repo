
# FastAPI ToDo API

A simple RESTful API for managing a task list using **FastAPI**, **SQLAlchemy**, **JWT auth**, and **PostgreSQL**.

## Features
- User registration & JWT-based authentication
- Task CRUD (owner-only update/delete)
- List current user's tasks with pagination and status filtering
- Admin-only endpoint to list **all** tasks
- Mark task as completed
- PostgreSQL config (Docker or local)
- Unit tests (pytest + FastAPI TestClient)

## Quickstart (Docker)
```bash
docker compose up --build
# API docs: http://localhost:8000/docs
```

## Local Dev (without Docker)
```bash
python -m venv .venv && source .venv/bin/activate    # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
# Ensure PostgreSQL is running and DATABASE_URL in .env is correct

# run api
uvicorn app.main:app --reload
```

### Environment Variables
- `DATABASE_URL` (e.g., `postgresql+psycopg2://todo:todo@localhost:5432/todo`)
- `JWT_SECRET` (random string)
- `ACCESS_TOKEN_EXPIRE_MINUTES` (default: 60)

## API Endpoints (summary)
- `POST /auth/register` – register user
- `POST /auth/login` – obtain JWT (OAuth2 password flow)
- `GET /tasks` – list **current user's** tasks; query: `status`, `limit`, `offset`
- `GET /tasks/all` – list all tasks (admin only)
- `POST /tasks` – create task
- `GET /tasks/{task_id}` – get task (owner or admin)
- `PUT /tasks/{task_id}` – update task (owner only)
- `DELETE /tasks/{task_id}` – delete task (owner only)
- `POST /tasks/{task_id}/complete` – mark as completed (owner only)

### Status values
- `New`, `In Progress`, `Completed`

## Running Tests
```bash
pytest -q
```

## GitHub Workflow
```bash
git init
git add .
git commit -m "feat: initial FastAPI ToDo API with JWT, Postgres, tests, docker"
git branch -M main
git remote add origin https://github.com/<your-username>/fastapi-todo.git
git push -u origin main
```

## Notes
- Tables are auto-created on startup. For production, consider Alembic migrations.
- The `/tasks/all` endpoint requires an admin user (set `is_admin` to `true` in DB for a user if needed).
- SQLite is used in tests for speed; runtime uses PostgreSQL when `DATABASE_URL` is set accordingly.
