# ts-api-starter

Scaffold a production-ready Express or Fastify REST API project in TypeScript.

## Instructions

When this skill is invoked, follow these steps exactly:

### Step 1 — Gather inputs

If the user did not provide a project name and/or framework as arguments, ask for them:

- **Project name**: a valid npm package name (lowercase, hyphens, no spaces)
- **Framework**: `express` or `fastify`

Use the AskUserQuestion tool to collect missing inputs before generating any files.

### Step 2 — Generate the project

Create all files below inside a new directory named after the project at the current working directory.

Both frameworks share the same directory layout:

```
{nom}/
├── src/
│   ├── index.ts          # serveur entry point
│   ├── app.ts            # app factory
│   └── routes/
│       └── health.ts     # GET /health
├── tests/
│   └── health.test.ts
├── package.json
├── tsconfig.json
├── Dockerfile
├── .gitignore
└── README.md
```

---

#### If framework = **express**

**`package.json`**
```json
{
  "name": "{PROJECT_NAME}",
  "version": "0.1.0",
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "test": "vitest run"
  },
  "dependencies": {
    "express": "^4.19.2"
  },
  "devDependencies": {
    "@types/express": "^4.17.21",
    "@types/node": "^20.14.0",
    "@types/supertest": "^6.0.2",
    "supertest": "^7.0.0",
    "ts-node": "^10.9.2",
    "typescript": "^5.4.5",
    "vitest": "^1.6.0"
  }
}
```
Replace `{PROJECT_NAME}` with the actual project name.

**`tsconfig.json`**
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "commonjs",
    "outDir": "dist",
    "rootDir": "src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src"]
}
```

**`src/app.ts`**
```typescript
import express, { Application } from "express";
import { healthRouter } from "./routes/health";

export function createApp(): Application {
  const app = express();
  app.use(express.json());
  app.use("/health", healthRouter);
  return app;
}
```

**`src/index.ts`**
```typescript
import { createApp } from "./app";

const PORT = process.env.PORT ?? 3000;
const app = createApp();

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

**`src/routes/health.ts`**
```typescript
import { Router } from "express";

export const healthRouter = Router();

healthRouter.get("/", (_req, res) => {
  res.json({ status: "ok" });
});
```

**`tests/health.test.ts`**
```typescript
import { describe, it, expect } from "vitest";
import request from "supertest";
import { createApp } from "../src/app";

describe("GET /health", () => {
  it("returns 200 with status ok", async () => {
    const app = createApp();
    const res = await request(app).get("/health");
    expect(res.status).toBe(200);
    expect(res.body).toEqual({ status: "ok" });
  });
});
```

**`Dockerfile`**
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

---

#### If framework = **fastify**

**`package.json`**
```json
{
  "name": "{PROJECT_NAME}",
  "version": "0.1.0",
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "test": "vitest run"
  },
  "dependencies": {
    "fastify": "^4.27.0"
  },
  "devDependencies": {
    "@types/node": "^20.14.0",
    "ts-node": "^10.9.2",
    "typescript": "^5.4.5",
    "vitest": "^1.6.0"
  }
}
```
Replace `{PROJECT_NAME}` with the actual project name.

**`tsconfig.json`**
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "commonjs",
    "outDir": "dist",
    "rootDir": "src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src"]
}
```

**`src/app.ts`**
```typescript
import Fastify, { FastifyInstance } from "fastify";
import { healthRoute } from "./routes/health";

export async function createApp(): Promise<FastifyInstance> {
  const app = Fastify({ logger: false });
  await app.register(healthRoute);
  return app;
}
```

**`src/index.ts`**
```typescript
import { createApp } from "./app";

const PORT = Number(process.env.PORT) || 3000;

createApp().then((app) => {
  app.listen({ port: PORT, host: "0.0.0.0" }, (err) => {
    if (err) {
      console.error(err);
      process.exit(1);
    }
    console.log(`Server running on port ${PORT}`);
  });
});
```

**`src/routes/health.ts`**
```typescript
import { FastifyInstance } from "fastify";

export async function healthRoute(app: FastifyInstance) {
  app.get("/health", async () => {
    return { status: "ok" };
  });
}
```

**`tests/health.test.ts`**
```typescript
import { describe, it, expect } from "vitest";
import { createApp } from "../src/app";

describe("GET /health", () => {
  it("returns 200 with status ok", async () => {
    const app = await createApp();
    const res = await app.inject({ method: "GET", url: "/health" });
    expect(res.statusCode).toBe(200);
    expect(res.json()).toEqual({ status: "ok" });
  });
});
```

**`Dockerfile`**
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

---

#### Commun aux deux frameworks

**`.gitignore`**
```
node_modules/
dist/
.env
*.js.map
```

**`README.md`**
```markdown
# {PROJECT_NAME}

API REST TypeScript avec {FRAMEWORK}.

## Démarrage

```bash
npm install
npm run dev
```

## Tests

```bash
npm test
```

## Build & production

```bash
npm run build
npm start
```

## Docker

```bash
docker build -t {PROJECT_NAME} .
docker run -p 3000:3000 {PROJECT_NAME}
```
```
Replace `{PROJECT_NAME}` and `{FRAMEWORK}` with the actual values.

### Step 3 — Confirmer

Après avoir créé tous les fichiers, affiche l'arborescence du projet généré et la commande de démarrage rapide.
