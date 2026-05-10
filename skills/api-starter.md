# api-starter

Scaffold a production-ready FastAPI or Flask REST API project.

## Instructions

When this skill is invoked, follow these steps exactly:

### Step 1 — Gather inputs

If the user did not provide a project name and/or framework as arguments, ask for them interactively:

- **Project name**: a valid Python package name (lowercase, underscores or hyphens, no spaces)
- **Framework**: `fastapi` or `flask`

Use the AskUserQuestion tool to collect missing inputs before generating any files.

### Step 2 — Generate the project

Create all files below inside a new directory named after the project at the current working directory. Do NOT create files outside that directory.

---

#### If framework = **fastapi**

**`requirements.txt`**
```
fastapi>=0.111.0
uvicorn[standard]>=0.29.0
pydantic>=2.7.0
pytest>=8.2.0
httpx>=0.27.0
pytest-asyncio>=0.23.0
```

**`app/__init__.py`** — empty

**`app/main.py`**
```python
from contextlib import asynccontextmanager
from fastapi import FastAPI
from app.api.routes import health


@asynccontextmanager
async def lifespan(app: FastAPI):
    yield


def create_app() -> FastAPI:
    app = FastAPI(title="{PROJECT_NAME}", lifespan=lifespan)
    app.include_router(health.router)
    return app


app = create_app()
```
Replace `{PROJECT_NAME}` with the actual project name.

**`app/api/__init__.py`** — empty

**`app/api/routes/__init__.py`** — empty

**`app/api/routes/health.py`**
```python
from fastapi import APIRouter

router = APIRouter(prefix="/health", tags=["health"])


@router.get("")
async def health_check():
    return {"status": "ok"}
```

**`app/schemas/__init__.py`** — empty

**`tests/__init__.py`** — empty

**`tests/test_main.py`**
```python
import pytest
from httpx import AsyncClient, ASGITransport
from app.main import app


@pytest.mark.asyncio
async def test_health():
    async with AsyncClient(transport=ASGITransport(app=app), base_url="http://test") as client:
        response = await client.get("/health")
    assert response.status_code == 200
    assert response.json() == {"status": "ok"}
```

**`Dockerfile`**
```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**`README.md`**
```markdown
# {PROJECT_NAME}

FastAPI REST API.

## Run locally

```bash
pip install -r requirements.txt
uvicorn app.main:app --reload
```

## Run with Docker

```bash
docker build -t {PROJECT_NAME} .
docker run -p 8000:8000 {PROJECT_NAME}
```

## Test

```bash
pytest
```
```
Replace `{PROJECT_NAME}` with the actual project name.

---

#### If framework = **flask**

**`requirements.txt`**
```
flask>=3.0.0
pytest>=8.2.0
pytest-flask>=1.3.0
```

**`app/__init__.py`**
```python
from flask import Flask
from app.routes.health import health_bp


def create_app() -> Flask:
    app = Flask(__name__)
    app.register_blueprint(health_bp)
    return app
```

**`app/routes/__init__.py`** — empty

**`app/routes/health.py`**
```python
from flask import Blueprint, jsonify

health_bp = Blueprint("health", __name__, url_prefix="/health")


@health_bp.get("")
def health_check():
    return jsonify({"status": "ok"})
```

**`tests/__init__.py`** — empty

**`tests/conftest.py`**
```python
import pytest
from app import create_app


@pytest.fixture
def client():
    app = create_app()
    app.config["TESTING"] = True
    with app.test_client() as client:
        yield client
```

**`tests/test_app.py`**
```python
def test_health(client):
    response = client.get("/health")
    assert response.status_code == 200
    assert response.get_json() == {"status": "ok"}
```

**`Dockerfile`**
```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000
CMD ["flask", "--app", "app:create_app", "run", "--host", "0.0.0.0", "--port", "5000"]
```

**`README.md`**
```markdown
# {PROJECT_NAME}

Flask REST API.

## Run locally

```bash
pip install -r requirements.txt
flask --app "app:create_app" run --debug
```

## Run with Docker

```bash
docker build -t {PROJECT_NAME} .
docker run -p 5000:5000 {PROJECT_NAME}
```

## Test

```bash
pytest
```
```
Replace `{PROJECT_NAME}` with the actual project name.

---

#### Common to both frameworks

**`.gitignore`**
```
__pycache__/
*.py[cod]
*.egg-info/
.venv/
venv/
.env
dist/
.pytest_cache/
.coverage
htmlcov/
```

### Step 3 — Confirm

After creating all files, print a tree of the generated structure and a quick-start command so the user can run the API immediately.
