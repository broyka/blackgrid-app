# Blackgrid — Design Task Management

A full‑stack web app for design task management with dark UI.

- Frontend: Next.js App Router, React, Tailwind CSS
- Backend: TypeScript, tRPC, Prisma, PostgreSQL
- Realtime: Socket.io (stubbed)
- Auth: Google OAuth via NextAuth
- Storage: S3‑compatible (stubs)
- Deploy: Vercel (frontend) + Fly.io/Render (backend-ready API routes)

## Quick Start

1) Install Node.js LTS (includes npm):
   https://nodejs.org/

2) Open a terminal in `blackgrid-app/` and install deps:

```bash
npm install
```

3) Copy environment file and fill values:

```bash
cp .env.example .env
```

Required vars:
- `DATABASE_URL` (PostgreSQL)
- `NEXTAUTH_SECRET` (any strong random string)
- `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`

4) Set up PostgreSQL and push schema:

```bash
npx prisma generate
npx prisma db push
```

5) Run the app:

```bash
npm run dev
```

Open http://localhost:3000

## Features Implemented

- Google SSO via `next-auth` (`src/pages/api/auth/[...nextauth].ts`)
- RBAC scaffolding for Client/Designer/Admin (`src/lib/trpc/server.ts`, `src/utils/rbac.ts`)
- Data model per spec in `prisma/schema.prisma`
- tRPC routers for tasks, users, orgs, chat (stubs) in `src/server/routers/`
- App layout with dark mode, sidebar and topbar
- Client dashboard with Request Task modal including EXACT task types
- Designer My Queue with Claim Task
- Admin and Plan pages scaffolded

## Important Paths

- `src/app/layout.tsx` — Root layout with Providers
- `src/components/Sidebar.tsx`, `src/components/Topbar.tsx`
- `src/components/modals/RequestTaskModal.tsx`
- `src/components/task/TaskBoard.tsx`
- `src/server/routers/*` — tRPC routers
- `src/pages/api/trpc/[trpc].ts` — tRPC handler
- `src/lib/auth.ts` — NextAuth config
- `src/lib/prisma.ts` — Prisma client singleton

## Status Automation (planned)

- New request -> In Queue
- Claim -> In Progress (sets assignee)
- Submit for review -> Waiting for Review
- Approve -> Done
- Auto archive after 14 days

These transitions are partially implemented in `taskRouter` and will be extended.

## Feature Flags

Read from `.env`:
- `FEATURE_CHAT_ENABLED`
- `FEATURE_BILLING_ENABLED`
- `FEATURE_NOTIFICATIONS_ENABLED`

Use them to conditionally render modules.

## Deploy

- Vercel (frontend): connect repo, set env vars
- Fly.io/Render (backend): This project uses Next API routes for simplicity; for large scale, split API into a separate Node service.

## Scripts

- `npm run dev` — start Next dev server
- `npm run build` — prod build
- `npm run start` — start prod server
- `npm run db:generate` — Prisma generate
- `npm run db:push` — Push schema to DB
- `npm run db:studio` — Prisma Studio

## Notes

- Chat and file uploads are stubbed. Wire S3 and Socket.io server as next steps.
- Ensure Google OAuth Consent Screen is configured and your `NEXTAUTH_URL` matches the dev URL.
