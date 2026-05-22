# NestJs Fastify React Full Stack Template

A reusable full-stack TypeScript application template with NestJS, Fastify,
React, PostgreSQL, Redis, Docker Compose, and observability built in. See
`CONTRIBUTING.md` for contribution guidance, `AGENTS.md` for conventions,
`docs/adr/` for recorded architectural decisions, and `docs/runbooks/` for
operational guides.

## Project scope

This template is intentionally kept minimal. It provides the foundation for a
full-stack service, but it does not try to include every capability a real
product usually needs.

There is no authentication, authorisation, user management, rate limiting,
abuse prevention, billing, background job system, email delivery, object
storage, feature flagging, admin area, or domain-specific workflow. Add those
pieces deliberately based on the product you are building, rather than carrying
generic defaults that may not fit.

## Tech stack

| Layer | Tech |
| --- | --- |
| Monorepo | pnpm workspaces, TypeScript |
| Backend | NestJS, Fastify |
| Frontend | Vite, React, Tailwind CSS, shadcn/ui |
| Database | PostgreSQL, Kysely, kysely-codegen |
| Cache | Redis |
| Validation and config | Zod |
| Testing | Vitest, Playwright browser mode, Testcontainers |
| Linting and formatting | Biome |
| Container runtime | Docker, Docker Compose |
| Observability | Prometheus, Grafana, Loki, Tempo, OpenTelemetry Collector |
| Git workflow | Lefthook, commitlint, conventional commits |

## Template cloning and project init

```bash
git clone https://github.com/dearzubi/nestjs-fastify-react-template.git my-app
cd my-app
bash scripts/init-project.sh acme
```

Replace `acme` with the package scope and infrastructure prefix you want for
the new project. The script updates workspace package names, local development
database placeholders, compose identifiers, and observability queries. It also
adds `docs/superpowers/` to `.git/info/exclude` for local AI-agent working
files.

## Quick start

### 1. Set up local tooling

```bash
nvm use
corepack enable
```

The Docker Compose stack uses the Loki Docker logging driver. Install it once
per Docker host. Use `3.7.0-arm64` on ARM64 hosts.

```bash
docker plugin install grafana/loki-docker-driver:3.7.0 --alias loki --grant-all-permissions
```

### 2. Install dependencies

```bash
pnpm install
```

### 3. Build the project

```bash
pnpm build
```

### 4. Prepare the root environment file

```bash
cp .env.example .env
```

The root `.env` is used by Docker Compose and is not committed.

Per-app env files (`apps/*/.env`, `apps/*/.env.development`) are committed
with safe defaults and are loaded automatically. Override locally with
`apps/*/.env.local` or `apps/*/.env.development.local` (both gitignored).

If the default Postgres host port is already in use, change `POSTGRES_PORT` in
the root `.env` and set `DATABASE_URL` in `apps/backend/.env.development.local`
to the same host port.

### 5. Run with local Node processes

Use Docker Compose for the supporting services, then run the backend and web app
through pnpm.

```bash
pnpm compose up -d postgres redis
pnpm db:migrate
pnpm dev
```

### 6. Run fully through Docker Compose

Use this when you want the app and supporting services to run in containers.
The Compose stack runs the one-shot `migrate` service before the backend starts.

```bash
pnpm compose up -d --build
```

## Single-VPS production

See [VPS production deployment](docs/runbooks/vps-production-deployment.md)
for the single-server production guide.
