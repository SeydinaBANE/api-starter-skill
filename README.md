# api-starter-skill

![GitHub stars](https://img.shields.io/github/stars/SeydinaBANE/api-starter-skill?style=flat-square)
![GitHub last commit](https://img.shields.io/github/last-commit/SeydinaBANE/api-starter-skill?style=flat-square)
![License](https://img.shields.io/github/license/SeydinaBANE/api-starter-skill?style=flat-square)
![Python](https://img.shields.io/badge/Python-3.12-blue?style=flat-square&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.4-3178C6?style=flat-square&logo=typescript&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.111-009688?style=flat-square&logo=fastapi&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.0-000000?style=flat-square&logo=flask&logoColor=white)
![Express](https://img.shields.io/badge/Express-4.19-000000?style=flat-square&logo=express&logoColor=white)
![Fastify](https://img.shields.io/badge/Fastify-4.27-000000?style=flat-square&logo=fastify&logoColor=white)

Une collection de skills Claude Code pour générer des APIs REST prêtes pour la production.

| Skill | Langage | Frameworks |
|---|---|---|
| `/api-starter` | Python | FastAPI, Flask |
| `/ts-api-starter` | TypeScript | Express, Fastify |

## Installation

```bash
npx skills add SeydinaBANE/api-starter-skill
```

---

## `/api-starter` — Python

Génère une API REST Python avec FastAPI ou Flask.

### Utilisation

```
/api-starter
/api-starter mon-projet fastapi
/api-starter mon-projet flask
```

### Structure générée (FastAPI)

```
mon-projet/
├── app/
│   ├── main.py
│   ├── api/routes/health.py   # GET /health
│   └── schemas/
├── tests/
│   └── test_main.py
├── requirements.txt
├── Dockerfile
└── .gitignore
```

### Structure générée (Flask)

```
mon-projet/
├── app/
│   ├── __init__.py            # create_app()
│   └── routes/health.py      # GET /health
├── tests/
│   ├── conftest.py
│   └── test_app.py
├── requirements.txt
├── Dockerfile
└── .gitignore
```

### Démarrage rapide

```bash
cd mon-projet
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
pytest
```

---

## `/ts-api-starter` — TypeScript

Génère une API REST TypeScript avec Express ou Fastify.

### Utilisation

```
/ts-api-starter
/ts-api-starter mon-projet express
/ts-api-starter mon-projet fastify
```

### Structure générée

```
mon-projet/
├── src/
│   ├── index.ts
│   ├── app.ts
│   └── routes/health.ts      # GET /health
├── tests/
│   └── health.test.ts
├── package.json
├── tsconfig.json
├── Dockerfile
└── .gitignore
```

### Démarrage rapide

```bash
cd mon-projet
npm install
npm test
npm run dev
```

---

## Licence

MIT
