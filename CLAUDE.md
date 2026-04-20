# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

Minimal Issue Tracker built with TypeScript + Hono + Prisma + PostgreSQL.
Primary goal: learn DDD layer separation (domain / usecase / infra / presentation) by hand-writing each layer.

## Commands

```bash
# Install dependencies
pnpm install

# Start PostgreSQL
docker compose up -d

# Run Prisma migrations
pnpm prisma migrate dev

# Start dev server
pnpm dev

# Type check
pnpm tsc --noEmit

# Lint & format
pnpm biome check .
pnpm biome check --write .

# Run all tests
pnpm vitest run

# Run a single test file
pnpm vitest run tests/usecase/issue/createIssue.test.ts

# Run integration tests only
pnpm vitest run tests/integration
```

## Architecture

Four-layer DDD with manual constructor injection (no DI container):

- **domain/** — Pure TypeScript types and business rules. Zero external dependencies (no Prisma types). Repository interfaces only (no implementations).
- **usecase/** — Business flow orchestration. Accesses data only through domain interfaces. One file per use case.
- **infra/** — External dependency implementations (DB via Prisma). Implements domain repository interfaces.
- **presentation/** — HTTP routing (Hono), request/response transformation, Zod validation. No domain logic here.

Dependency direction: `presentation → usecase → domain ← infra`

## Key Design Decisions

- **Anemic Domain Model** (Phase 1a): Entity has no domain methods yet — intentional starting point before Rich Domain Model in later phases.
- **Manual DI**: Dependencies wired in `src/container.ts` via constructor injection. No Inversify or similar.
- **Status as string**: `Issue.status` stored as plain string (`"open"`, etc.), enum conversion planned for Phase 1b.
- **Fake Repository for tests**: No mock libraries — hand-written in-memory fakes at `tests/fakes/fakeIssueRepository.ts`.

## Tech Stack

| Area | Choice |
|------|--------|
| Runtime | Node.js 22 LTS |
| Framework | Hono |
| ORM | Prisma |
| DB | PostgreSQL 16 (Docker) |
| Test | Vitest |
| Validation | Zod |
| Package manager | pnpm |
| Linter/Formatter | Biome |

**Not used**: fp-ts, Inversify, TypeORM, Jest, ESLint+Prettier.

## Mentoring Mode

This project is a learning exercise. Claude's role is **mentor, not implementor**:
- Never generate code without explaining the design intent first and getting user consent.
- One file per step — always pause for understanding confirmation.
- Ask the user to explain each layer's responsibility in their own words before moving to the next layer.
- When multiple design options exist, present them with trade-offs rather than picking one silently.
