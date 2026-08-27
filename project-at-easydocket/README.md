# DocketFlow

Multi-tenant construction docketing SaaS for Australian building and construction workflows. Field workers capture completed work and client signatures on site; administrators generate progress claims with retention under the Security of Payment Act (NSW).

## User guide

For day-to-day setup and use (Administrator, Supervisor, Field Technician), see **[USER_GUIDE.md](USER_GUIDE.md)**.

## Stack

- **Frontend:** React + Vite + PWA (offline dockets)
- **Hosting:** Cloudflare Pages
- **Backend:** Supabase (Postgres, Auth, Storage, Edge Functions)
- **CI/CD:** GitHub Actions

## Local development

### Prerequisites

- Node.js 22+
- [Supabase CLI](https://supabase.com/docs/guides/cli)
- Docker (for local Supabase)

### Setup

```bash
npm install
cp .env.example .env.local
supabase start
# Copy anon key and URL from `supabase status` into .env.local
supabase db reset
npm run dev
```

In a **second terminal**, serve edge functions (required for invites, platform admin, PDF export):

```bash
npm run functions:serve
```

> After adding new functions, restart Supabase (`supabase stop && supabase start`) or run `npm run functions:serve` so they are available at `http://127.0.0.1:54321/functions/v1/`.

### Bootstrap users

**Platform super admin** (control plane — manages tenants):

```bash
cp .env.platform.example .env.platform
# Paste URL + Secret key from `npx supabase status`
npm run seed:platform-admin
# Log in at http://localhost:5173/platform/login
```

**Demo tenant users** (optional, for testing the tenant app):

```bash
npx tsx scripts/seed-users.ts
# Log in at http://localhost:5173/login with admin@demo.local / demo123456
```

### Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start Vite dev server |
| `npm run build` | Production build |
| `npm test` | Run unit tests |
| `npm run lint` | Run oxlint |
| `npm run seed:platform-admin` | Bootstrap platform super admin |
| `npm run seed:users` | Create local demo tenant users |
| `npm run functions:serve` | Serve edge functions locally (run alongside `dev`) |

## Deployment

See **[DEPLOY.md](DEPLOY.md)** for the full step-by-step guide to GitHub, Supabase, and Cloudflare Pages.

Quick start after creating accounts:

```bash
./scripts/setup-deploy.sh    # interactive guided setup
# or
./scripts/deploy-supabase.sh # migrations + functions only
```

### Cloudflare Pages

1. Connect this repo to Cloudflare Pages
2. Build command: `npm run build`
3. Output directory: `dist`
4. Set environment variables:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`

### Supabase (production)

1. Create a project in **ap-southeast-2 (Sydney)**
2. Add GitHub secrets:
   - `SUPABASE_ACCESS_TOKEN`
   - `SUPABASE_PROJECT_REF`
   - `SUPABASE_DB_PASSWORD`
3. Push to `main` — migrations deploy via `.github/workflows/deploy-supabase.yml`
4. Edge functions deploy via `.github/workflows/deploy-functions.yml` (or `./scripts/deploy-supabase.sh`)
5. Bootstrap super admin: `npm run seed:platform-admin` (see DEPLOY.md)

## Architecture

- **Multi-tenancy:** `tenant_id` on all tables + RLS policies; tenant status (`active`, `read_only`, `disabled`)
- **Platform layer:** Super admin manages tenant lifecycle via `/platform/*` (separate from tenant data)
- **Tenant roles:** Administrator, Project Manager, Field Worker
- **Offline:** PWA + IndexedDB sync queue for signed dockets
- **Capacitor:** Planned migration path for native mobile (see `capacitor.config.ts`)

## Security

- Platform super admin isolated from tenant business data (control plane / data plane split)
- Signed dockets are immutable after finalization
- Signatures stored in private Supabase Storage bucket
- Local offline data encrypted with Web Crypto API
- Tenant audit log for admin visibility; platform audit log for super admin actions

## Legal

SoP Act payment due dates are calculated as guidance only. Obtain legal review before production billing use.
