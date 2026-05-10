# api-starter

Un skill Claude Code qui génère en quelques secondes une **FastAPI** ou **Flask** REST API prête pour la production.

## Installation

```bash
npx skills add <ton-username-github>/api-starter
```

## Utilisation

Dans n'importe quelle session Claude Code, tape :

```
/api-starter
```

Claude te demandera le **nom du projet** et le **framework** (FastAPI ou Flask), puis générera toute la structure du projet.

Tu peux aussi passer les arguments directement :

```
/api-starter mon-projet fastapi
/api-starter mon-projet flask
```

## Ce qui est généré

### FastAPI

```
mon-projet/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── api/
│   │   └── routes/
│   │       └── health.py   # GET /health
│   └── schemas/
│       └── __init__.py
├── tests/
│   └── test_main.py
├── requirements.txt
├── Dockerfile
├── .gitignore
└── README.md
```

### Flask

```
mon-projet/
├── app/
│   ├── __init__.py         # factory create_app()
│   └── routes/
│       └── health.py       # GET /health
├── tests/
│   ├── conftest.py
│   └── test_app.py
├── requirements.txt
├── Dockerfile
├── .gitignore
└── README.md
```

## Démarrage rapide après le scaffold

```bash
cd mon-projet
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
pytest
```

## Licence

MIT
