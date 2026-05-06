# ⚡ NodeForge — High-Performance Node.js API Boilerplate

<div align="center">

![NodeForge Banner](https://images.unsplash.com/photo-1558494949-ef010cbdcc31?w=1200&h=400&fit=crop&q=80)

[![Node.js Version](https://img.shields.io/badge/node-%3E%3D20.0.0-brightgreen?style=for-the-badge&logo=node.js)](https://nodejs.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](http://makeapullrequest.com)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)
[![CI](https://img.shields.io/github/actions/workflow/status/yourorg/nodeforge/ci.yml?style=for-the-badge&label=CI)](https://github.com/yourorg/nodeforge/actions)

**Production-ready Node.js REST API boilerplate powered by Express, TypeScript, Redis, and PostgreSQL.**  
Built for scale. Designed for developer happiness.

[Getting Started](#-getting-started) · [Documentation](#-documentation) · [Architecture](#-architecture) · [Contributing](#-contributing) · [Changelog](CHANGELOG.md)

</div>

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
- [Project Structure](#-project-structure)
- [API Reference](#-api-reference)
- [Architecture](#-architecture)
- [Testing](#-testing)
- [Deployment](#-deployment)
- [Performance Benchmarks](#-performance-benchmarks)
- [Contributing](#-contributing)
- [License](#-license)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔐 **JWT Authentication** | Access + refresh token rotation with Redis blacklisting |
| 🛡️ **Role-Based Access Control** | Granular permission system with middleware guards |
| 📦 **Database ORM** | Prisma with PostgreSQL, auto-migrations, seed scripts |
| 🔁 **Job Queues** | BullMQ-powered background workers with retry strategies |
| 🧪 **Full Test Coverage** | Unit, integration, and E2E tests via Vitest + Supertest |
| 📊 **Observability** | Structured logging (Pino), metrics (Prometheus), tracing (OTEL) |
| 🚦 **Rate Limiting** | Sliding-window rate limits backed by Redis |
| 📄 **Auto Docs** | OpenAPI 3.1 spec auto-generated from route definitions |
| 🐳 **Docker Ready** | Multi-stage Dockerfile + Compose for local dev |
| 🔄 **Graceful Shutdown** | SIGTERM-aware shutdown with in-flight request draining |

---

## 🛠 Tech Stack

<div align="center">

![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=flat-square&logo=prisma&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Vitest](https://img.shields.io/badge/Vitest-6E9F18?style=flat-square&logo=vitest&logoColor=white)

</div>

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:

- **Node.js** `>= 20.0.0` ([nvm recommended](https://github.com/nvm-sh/nvm))
- **pnpm** `>= 9.0.0`
- **Docker** + **Docker Compose** (for local services)
- **PostgreSQL** `>= 15` (or use Docker)
- **Redis** `>= 7` (or use Docker)

```bash
# Check your Node.js version
node --version  # should output v20.x.x or higher

# Install pnpm if you haven't already
npm install -g pnpm
```

---

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/yourorg/nodeforge.git
cd nodeforge

# 2. Install dependencies
pnpm install

# 3. Spin up local services (Postgres + Redis)
docker compose up -d db redis

# 4. Copy and configure environment variables
cp .env.example .env

# 5. Run database migrations and seed data
pnpm db:migrate
pnpm db:seed

# 6. Start the development server with hot-reload
pnpm dev
```

> 🎉 The API is now running at **http://localhost:3000**  
> 📚 Swagger UI is available at **http://localhost:3000/docs**

---

### Environment Variables

Create a `.env` file at the root of the project. All required variables are documented below:

```env
# ─── Application ─────────────────────────────────────────────
NODE_ENV=development
PORT=3000
APP_NAME=NodeForge
API_VERSION=v1

# ─── Database ────────────────────────────────────────────────
DATABASE_URL=postgresql://postgres:secret@localhost:5432/nodeforge_dev
DATABASE_POOL_MIN=2
DATABASE_POOL_MAX=10

# ─── Redis ───────────────────────────────────────────────────
REDIS_URL=redis://localhost:6379
REDIS_TLS=false

# ─── Authentication ──────────────────────────────────────────
JWT_ACCESS_SECRET=your-super-secret-access-key-change-me
JWT_REFRESH_SECRET=your-super-secret-refresh-key-change-me
JWT_ACCESS_EXPIRES_IN=15m
JWT_REFRESH_EXPIRES_IN=7d

# ─── Rate Limiting ───────────────────────────────────────────
RATE_LIMIT_WINDOW_MS=60000
RATE_LIMIT_MAX_REQUESTS=100

# ─── Email (Resend) ──────────────────────────────────────────
RESEND_API_KEY=re_xxxxxxxxxxxxxxxxxxxx
EMAIL_FROM=no-reply@yourdomain.com

# ─── Storage (S3-compatible) ─────────────────────────────────
S3_ENDPOINT=https://s3.amazonaws.com
S3_BUCKET=nodeforge-uploads
S3_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
S3_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
S3_REGION=us-east-1
```

> ⚠️ **Never commit your `.env` file.** It is listed in `.gitignore` by default.

---

## 📁 Project Structure

```
nodeforge/
├── src/
│   ├── config/            # App configuration (env validation, constants)
│   │   ├── env.ts
│   │   └── database.ts
│   ├── modules/           # Feature modules (vertical slice architecture)
│   │   ├── auth/
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   ├── auth.router.ts
│   │   │   ├── auth.schema.ts     # Zod validation schemas
│   │   │   └── auth.test.ts
│   │   ├── users/
│   │   │   ├── user.controller.ts
│   │   │   ├── user.service.ts
│   │   │   ├── user.repository.ts
│   │   │   ├── user.router.ts
│   │   │   └── user.test.ts
│   │   └── uploads/
│   ├── middleware/        # Express middleware
│   │   ├── auth.middleware.ts
│   │   ├── error.middleware.ts
│   │   ├── rateLimit.middleware.ts
│   │   └── validate.middleware.ts
│   ├── workers/           # BullMQ background jobs
│   │   ├── email.worker.ts
│   │   └── cleanup.worker.ts
│   ├── lib/               # Shared utilities & service clients
│   │   ├── logger.ts
│   │   ├── redis.ts
│   │   ├── prisma.ts
│   │   └── queue.ts
│   ├── types/             # Global TypeScript types
│   └── app.ts             # Express app factory
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts
├── tests/
│   ├── fixtures/
│   ├── helpers/
│   └── e2e/
├── docker/
│   ├── Dockerfile
│   └── docker-compose.yml
├── .env.example
├── tsconfig.json
├── vitest.config.ts
└── package.json
```

---

## 📡 API Reference

### Authentication

#### Register a new user

```http
POST /api/v1/auth/register
Content-Type: application/json

{
  "email": "jane@example.com",
  "password": "Str0ng!Pass",
  "name": "Jane Doe"
}
```

**Response `201 Created`:**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_01HZ9K3MXNTQ8BAPYZ6FVTD4RW",
      "email": "jane@example.com",
      "name": "Jane Doe",
      "role": "USER",
      "createdAt": "2025-01-15T10:30:00.000Z"
    },
    "tokens": {
      "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "expiresIn": 900
    }
  }
}
```

#### Login

```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "jane@example.com",
  "password": "Str0ng!Pass"
}
```

#### Refresh Token

```http
POST /api/v1/auth/refresh
Content-Type: application/json

{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### Logout (invalidates token)

```http
POST /api/v1/auth/logout
Authorization: Bearer <accessToken>
```

---

### Users

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `GET` | `/api/v1/users` | Admin | List all users (paginated) |
| `GET` | `/api/v1/users/:id` | User | Get user by ID |
| `PATCH` | `/api/v1/users/:id` | User | Update user profile |
| `DELETE` | `/api/v1/users/:id` | Admin | Soft-delete a user |

**Paginated list example:**

```http
GET /api/v1/users?page=1&limit=20&sort=createdAt&order=desc
Authorization: Bearer <accessToken>
```

```json
{
  "success": true,
  "data": {
    "users": [...],
    "pagination": {
      "total": 342,
      "page": 1,
      "limit": 20,
      "totalPages": 18
    }
  }
}
```

---

## 🏗 Architecture

### Request Lifecycle

```
Client Request
      │
      ▼
  [ Rate Limiter ] ──(exceeded)──► 429 Too Many Requests
      │
      ▼
  [ Auth Middleware ] ──(invalid)──► 401 Unauthorized
      │
      ▼
  [ Request Validator (Zod) ] ──(invalid)──► 422 Unprocessable Entity
      │
      ▼
  [ Controller ]
      │
      ▼
  [ Service Layer ] ◄──► [ Repository Layer ] ◄──► [ PostgreSQL ]
      │                          │
      │                          └──► [ Redis Cache ]
      │
      ▼
  [ Response Transformer ]
      │
      ▼
  JSON Response
```

### Database Schema (simplified)

```prisma
// prisma/schema.prisma

model User {
  id           String    @id @default(cuid())
  email        String    @unique
  name         String
  passwordHash String
  role         Role      @default(USER)
  isActive     Boolean   @default(true)
  sessions     Session[]
  createdAt    DateTime  @default(now())
  updatedAt    DateTime  @updatedAt
  deletedAt    DateTime?

  @@index([email])
  @@map("users")
}

model Session {
  id           String   @id @default(cuid())
  userId       String
  refreshToken String   @unique
  userAgent    String?
  ipAddress    String?
  expiresAt    DateTime
  createdAt    DateTime @default(now())
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@index([userId])
  @@map("sessions")
}

enum Role {
  USER
  ADMIN
  SUPERADMIN
}
```

---

## 🧪 Testing

NodeForge uses **Vitest** for all tests with `@testcontainers/postgresql` for integration tests — no mocks for DB, real containers.

```bash
# Run all tests
pnpm test

# Watch mode (for TDD)
pnpm test:watch

# Integration tests only (spins up test DB)
pnpm test:integration

# E2E tests
pnpm test:e2e

# Coverage report
pnpm test:coverage
```

**Coverage targets** (enforced in CI):

| Metric | Threshold |
|--------|-----------|
| Statements | 85% |
| Branches | 80% |
| Functions | 85% |
| Lines | 85% |

**Example test:**

```typescript
// src/modules/auth/auth.test.ts
import { describe, it, expect, beforeAll, afterAll } from 'vitest'
import request from 'supertest'
import { app } from '../../app'
import { prisma } from '../../lib/prisma'

describe('POST /api/v1/auth/register', () => {
  afterAll(async () => {
    await prisma.user.deleteMany({ where: { email: { contains: 'test-' } } })
  })

  it('should register a new user and return tokens', async () => {
    const res = await request(app)
      .post('/api/v1/auth/register')
      .send({
        email: 'test-jane@example.com',
        password: 'Str0ng!Pass',
        name: 'Test Jane',
      })

    expect(res.status).toBe(201)
    expect(res.body.success).toBe(true)
    expect(res.body.data.tokens.accessToken).toBeDefined()
    expect(res.body.data.user.passwordHash).toBeUndefined() // never exposed
  })

  it('should return 409 if email already exists', async () => {
    // Register twice with same email
    await request(app).post('/api/v1/auth/register').send({
      email: 'test-duplicate@example.com',
      password: 'Str0ng!Pass',
      name: 'Dup User',
    })

    const res = await request(app).post('/api/v1/auth/register').send({
      email: 'test-duplicate@example.com',
      password: 'Str0ng!Pass',
      name: 'Dup User',
    })

    expect(res.status).toBe(409)
  })
})
```

---

## 🐳 Deployment

### Docker (Production)

The included `Dockerfile` uses a multi-stage build for minimal image size:

```bash
# Build the production image
docker build -t nodeforge:latest .

# Run with environment variables
docker run -d \
  --name nodeforge \
  -p 3000:3000 \
  --env-file .env.production \
  nodeforge:latest
```

**Image sizes:**

| Stage | Size |
|-------|------|
| Builder | ~1.2 GB |
| Final (distroless) | ~180 MB |

### Docker Compose (Full Stack)

```bash
# Start all services (app + db + redis + adminer)
docker compose -f docker/docker-compose.yml up -d

# View logs
docker compose logs -f app

# Stop everything
docker compose down -v
```

### Kubernetes

Helm chart available in the `/helm` directory:

```bash
helm repo add nodeforge https://charts.yourorg.dev
helm install nodeforge nodeforge/nodeforge \
  --namespace production \
  --set image.tag=1.4.2 \
  --set replicaCount=3 \
  -f values.production.yaml
```

### CI/CD

The project ships with a **GitHub Actions** pipeline (`.github/workflows/ci.yml`) that:

1. Lints and type-checks on every PR
2. Runs the full test suite with Postgres + Redis service containers
3. Builds and pushes the Docker image to GHCR on merge to `main`
4. Deploys to staging automatically; production requires manual approval

---

## 📈 Performance Benchmarks

Tested on: `AWS t3.medium` (2 vCPU / 4GB RAM), Node.js 20, `--max-old-space-size=512`

```
autocannon -c 100 -d 30 http://localhost:3000/api/v1/health

Running 30s test @ http://localhost:3000/api/v1/health
100 connections

┌─────────┬──────┬──────┬───────┬──────┬─────────┬─────────┬────────┐
│ Stat    │ 2.5% │ 50%  │ 97.5% │ 99%  │ Avg     │ Stdev   │ Max    │
├─────────┼──────┼──────┼───────┼──────┼─────────┼─────────┼────────┤
│ Latency │ 1 ms │ 2 ms │ 6 ms  │ 9 ms │ 2.14 ms │ 1.32 ms │ 58 ms  │
└─────────┴──────┴──────┴───────┴──────┴─────────┴─────────┴────────┘

Req/Bytes counts sampled once per second.

41k requests in 30.04s, 8.87 MB read
Requests/sec: 1,364.87
Transfer/sec: 302.18 kB
```

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting a PR.

```bash
# Fork, then clone your fork
git clone https://github.com/<your-username>/nodeforge.git

# Create a feature branch
git checkout -b feat/my-awesome-feature

# Make your changes, add tests, commit
git commit -m "feat: add awesome feature (#42)"

# Push and open a PR
git push origin feat/my-awesome-feature
```

### Commit Convention

This project follows [Conventional Commits](https://www.conventionalcommits.org/):

```
feat:     A new feature
fix:      A bug fix
docs:     Documentation changes
style:    Formatting (no logic change)
refactor: Code restructuring
test:     Adding or updating tests
chore:    Maintenance tasks
```

---

## 📄 License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for more information.

---

<div align="center">

Made with ❤️ and ☕ by the NodeForge team

[![GitHub Stars](https://img.shields.io/github/stars/yourorg/nodeforge?style=social)](https://github.com/yourorg/nodeforge)
[![Follow on X](https://img.shields.io/twitter/follow/yourorg?style=social)](https://twitter.com/yourorg)

</div>
