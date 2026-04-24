# LeadNova

A polished mini-CRM for small B2B teams to capture inbound leads, move them through new → contacted → converted, and track notes.

## Architecture

Monorepo (pnpm) with shared OpenAPI codegen.

- **artifacts/api-server** — Express + Drizzle ORM + PostgreSQL backend
  - JWT (jsonwebtoken) + bcryptjs auth
  - Routes: `/auth/login`, `/auth/me`, `/leads` (CRUD + search/filter), `/leads/:id/notes`, `/analytics/summary`, `/analytics/recent-activity`
  - Auto-seeds `admin@leadnova.com` / `admin123` and 5 sample leads on first boot
- **artifacts/leadnova** — React + Vite + wouter + TanStack Query frontend (shadcn/ui)
  - Pages: `/login`, `/dashboard`, `/leads`, `/leads/:id`
  - Auth context stores JWT in `localStorage` under `leadnova_token` and wires `setAuthTokenGetter`
- **lib/api-spec** — OpenAPI source of truth; codegen produces typed hooks
- **lib/api-client-react** — Generated React Query hooks
- **lib/api-zod** — Generated Zod request/response schemas (used for validation in the API)
- **lib/db** — Drizzle schema (`users`, `leads`, `lead_notes`) and DB client

## Key conventions

- Auth: JWT secret falls back to `JWT_SECRET` → `SESSION_SECRET` → dev secret.
- Adding/changing API: edit `lib/api-spec/openapi.yaml`, run `pnpm --filter @workspace/api-spec run codegen`.
- DB schema changes: edit `lib/db/src/schema/index.ts`, run `pnpm --filter @workspace/db run db:push`.

## Demo credentials

- Email: `admin@leadnova.com`
- Password: `admin123`
