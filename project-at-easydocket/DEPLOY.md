# Deployment Guide — DocketFlow

Step-by-step to deploy to GitHub, Supabase (Sydney), and Cloudflare Pages.

## Architecture

| Layer | Hosts | Deploy trigger |
|-------|-------|----------------|
| **GitHub** | Source code, CI/CD workflows | `git push origin main` |
| **Cloudflare Pages** | Built React SPA (`dist/`) | Auto on push to `main` |
| **Supabase** | Postgres, Auth, Storage, Edge Functions | GitHub Actions (migrations + functions) or `./scripts/deploy-supabase.sh` |

**Roles:**

- **Platform super admin** (`/platform/login`) — manages tenant lifecycle only; no access to tenant business data
- **Tenant administrator** (`/login`) — manages users and organization settings within one tenant
- **Project manager / field worker** — tenant-scoped operational access

Bootstrap on a fresh install: run `npm run seed:platform-admin` once, then create tenants via the platform UI.
After wiping tenant data (or truncating lookup tables), run `npm run seed:reference-data` to restore `roles` / `job_statuses` / `billing_types` and sync job, docket, and tenant-number counters.

## Prerequisites checklist

- [ ] GitHub account + repo `at-easydocket`
- [ ] Supabase account + project in **ap-southeast-2 (Sydney)**
- [ ] Cloudflare account
- [ ] Supabase CLI logged in: `npx supabase login`
- [ ] Node.js 22+

---

## Versioning and changelog

We use [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`) in `package.json`. The left-nav footer shows `vMAJOR.MINOR.PATCH · gitSha` (baked in at build time).

**Every production commit increments PATCH** so a user can tell the deployed app is newer (`v0.3.1` → `v0.3.2` → `v0.3.3`). The git SHA still identifies the exact commit; tell users the **version**, not the hash.

| When | Command | Example |
|------|---------|---------|
| Default — every shipped commit (fix or small change) | `npm run version:patch` | `0.3.1` → `0.3.2` |
| New feature set (only when you choose) | `npm run version:minor` | `0.3.2` → `0.4.0` |
| Breaking change (only when you choose) | `npm run version:major` | `0.4.0` → `1.0.0` |

Workflow (required on every commit to `main`):

1. Add a bullet under `## [Unreleased]` in `CHANGELOG.md` describing the change.
2. Run `npm run version:patch` (or `version:minor` / `version:major` if you explicitly want a larger bump).
3. Commit `package.json` + `CHANGELOG.md` **in the same commit** as the rest of the work, then `git push`.

Cursor agents follow this automatically (project rule `.cursor/rules/version-bump-on-commit.mdc`) when you ask them to commit. If you commit in the terminal yourself, run step 2 before `git commit`.

Empty Unreleased notes are rejected unless you pass `--force` (e.g. `npm run version:patch -- --force`).

History lives in `CHANGELOG.md` in the repo; optionally publish the same notes as a [GitHub Release](https://docs.github.com/en/repositories/releasing-projects-on-github) when tagging.

---

## Step 1 — Push to GitHub

```bash
git add .
git commit -m "Your commit message"
git push origin main
```

CI runs automatically (`.github/workflows/ci.yml`).

---

## Step 2 — Supabase production

### 2a. Create project

1. [supabase.com/dashboard](https://supabase.com/dashboard) → New project
2. Region: **Sydney (ap-southeast-2)**
3. Save database password

### 2b. Apply migrations + functions

**Option A — script (recommended for first deploy):**

```bash
export SUPABASE_PROJECT_REF=your-project-ref   # from Project Settings → General
export SUPABASE_DB_PASSWORD=your-db-password
./scripts/deploy-supabase.sh
```

**Option B — GitHub Actions (ongoing):**

After GitHub secrets are configured (Step 4), pushes to `main` that change `supabase/**` auto-deploy via:

- `.github/workflows/deploy-supabase.yml` — migrations
- `.github/workflows/deploy-functions.yml` — edge functions

**Edge functions deployed:**

| Function | Purpose |
|----------|---------|
| `invite-user` | Tenant admin invites users (legacy) |
| `create-user` | Tenant admin creates users (password or invite) |
| `set-user-active` | Tenant admin activates/deactivates users (revokes sessions on deactivate) |
| `manage-tenant-user` | Tenant admin set password, resend invite, or send password recovery |
| `export-claim-pdf` | Progress claim PDF export |
| `send-email` | Async email sending pipeline for docket sharing (updates `email_log`, retries, provider adapter) |
| `platform-tenants` | List / create tenants |
| `platform-tenant` | Update tenant name / status / email_enabled |
| `platform-tenant-admin` | Create / reset tenant administrator |

**Migrations required for the email + locale features:**

- `20260101000035_email_system.sql` — `platform_email_config`, `email_templates`, `email_recipient_settings`, `email_log` (RLS + realtime)
- `20260101000036_docket_pdfs_bucket.sql` — private `docket-pdfs` bucket for docket email attachments
- `20260101000037_tenant_locale_settings.sql` — `tenants.timezone`, `date_format`, `time_format`, `number_locale`
- `20260101000038_email_feature_toggle.sql` — `tenants.email_enabled`, `tenant_email_config` (per-tenant provider credentials), platform-admin RLS

### 2c. Auth configuration (Dashboard)

**Authentication → URL Configuration**

| Setting | Value |
|---------|-------|
| Site URL | `https://YOUR_APP.pages.dev` |
| Redirect URLs | See list below |

**Production redirect URLs** (replace with your Pages URL):

```
https://YOUR_APP.pages.dev/**
https://YOUR_APP.pages.dev/set-password
https://YOUR_APP.pages.dev/platform/login
http://localhost:5173/**
http://localhost:5173/set-password
http://127.0.0.1:5173/**
http://127.0.0.1:5173/set-password
```

Or run: `./scripts/print-auth-config.sh https://YOUR_APP.pages.dev`

**Authentication → Providers → Email**

| Setting | Value |
|---------|-------|
| Enable Email provider | ON |
| Confirm email | ON (production) |
| Disable new signups | ON (invite-only) |

**Authentication → Email Templates → Invite user**

Customize the Invite template so invitation emails include DocketFlow branding, organisation name, and role. Edge Functions pass metadata used by `{{ .Data.* }}` (`firm_name`, `role_label`, `full_name`). Redeploy `create-user`, `invite-user`, and `platform-tenant-admin` after code changes that alter that metadata.

Suggested subject:

```
You're invited to DocketFlow — {{ .Data.firm_name }}
```

Suggested body:

```html
<h2>You're invited to DocketFlow</h2>
<p>Hello{{ if .Data.full_name }} {{ .Data.full_name }}{{ end }},</p>
<p>You've been invited to join <strong>{{ .Data.firm_name }}</strong> on DocketFlow as a <strong>{{ .Data.role_label }}</strong>.</p>
<p>Use the button below to accept the invitation and set your password. This link expires after a limited time and can only be used once.</p>
<p><a href="{{ .ConfirmationURL }}">Accept invitation</a></p>
<p>If you weren't expecting this email, you can ignore it.</p>
```

Apply the same template on each Supabase project (Dev and Prod) that sends invites.

### 2d. Platform super admin (fresh install)

```bash
cp .env.platform.example .env.platform
# Edit with PRODUCTION Supabase URL, Secret (service role) key, super admin email/password
npm run seed:platform-admin
```

Log in at `https://YOUR_PAGES_URL/platform/login`, then create tenants and tenant administrators from the platform console.

> **Note:** `seed:prod` and `supabase/seed-prod.sql` are deprecated. Use the platform admin UI to provision tenants.

### 2e. Clean redeploy / data wipe

**Preferred — wipe script** (deletes all app data + auth users, restores lookups, resets `tenant_number`):

```bash
# Local (uses `supabase status` credentials)
npm run db:wipe -- --local
# type: WIPE LOCAL

# Production (uses .env.platform; project must be `supabase link`ed)
npm run db:wipe -- --prod
# type: WIPE PRODUCTION
# or non-interactive: CONFIRM='WIPE PRODUCTION' npm run db:wipe -- --prod
```

Then bootstrap again:

```bash
npm run seed:platform-admin
# Local demo users only:
npm run seed:users
```

**Manual alternative** if replacing an old setup without the script:

1. Deploy migrations (Step 2b)
2. Remove users / truncate data carefully in SQL Editor
3. **Do not leave lookup tables empty** (`roles`, `job_statuses`, `billing_types`, rate-item lookups) — or restore with `npm run seed:reference-data`
4. Run `npm run seed:platform-admin`
5. Recreate tenants / administrators via the platform UI (or `npm run seed:users` locally)
---

## Step 3 — Cloudflare Pages

1. [dash.cloudflare.com](https://dash.cloudflare.com) → Workers & Pages → Create → Pages → Connect to Git
2. Select `at-easydocket` repo
3. Build settings:
   - Framework preset: React (Vite)
   - Build command: `npm run build`
   - Output directory: `dist`
4. Environment variables (Production):

| Name | Value |
|------|-------|
| `VITE_SUPABASE_URL` | `https://YOUR_PROJECT_REF.supabase.co` |
| `VITE_SUPABASE_ANON_KEY` | Publishable key from Supabase → API Keys |

> Important: Do NOT add a `wrangler.toml` to this project. When present, Cloudflare
> Pages ignores dashboard environment variables at build time (build log shows
> `Build environment variables: (none found)`), and the Vite build ships without
> Supabase config, producing a white screen. This is a static SPA with no
> Cloudflare Functions, so `wrangler.toml` is not needed.

Vite bakes `VITE_*` values in at build time, so after adding or changing them you
must trigger a new deployment (Deployments → Retry deployment, or push a commit).

5. Deploy → copy your `*.pages.dev` URL
6. Update Supabase Auth Site URL and redirect URLs to match (Step 2c)

---

## Step 4 — GitHub Actions secrets

Repo → Settings → Secrets and variables → Actions → New repository secret:

| Secret | Source |
|--------|--------|
| `SUPABASE_ACCESS_TOKEN` | [Account tokens](https://supabase.com/dashboard/account/tokens) |
| `SUPABASE_PROJECT_REF` | Project ref from URL |
| `SUPABASE_DB_PASSWORD` | Database password from project creation |

After this, pushes to `main` that change `supabase/**` auto-deploy migrations and functions.

---

## Step 5 — Verify

- [ ] GitHub Actions CI green
- [ ] Cloudflare deployment successful
- [ ] Platform login at `/platform/login` works
- [ ] Create a tenant and tenant administrator from platform console
- [ ] Platform super admin configures **Email provider (tenant-specific)** and **Email feature** toggle in `/platform/tenants/:id` (provider, From email/name, reply-to mode, max retries, provider credentials; enable/disable email per tenant)
- [ ] Tenant admin login at `/login` works (forced password change on first login)
- [ ] Tenant admin can invite users and manage organization settings (`/admin/tenant-profile` via `/admin/settings`)
- [ ] When email is **enabled** for a tenant: admin can set **Time Zone & Locale** and **Email Settings** (routes: `/admin/tenant-profile`, `/admin/email-settings`)
- [ ] When email is **disabled** for a tenant: Email Settings, Share, Email log, and Notifications UI are hidden; queued emails do not send
- [ ] Sharing a submitted docket (when email enabled) triggers email send and shows status in Notifications / Email log (offline share queues and sends when online), and CC includes all configured recipients from Email Settings (including “Other Recipients”)
- [ ] Tenant status changes (active / read-only / disabled) take effect immediately

---

## Ongoing

| Change | Auto-deploy |
|--------|-------------|
| Frontend (`src/**`) | Cloudflare on push to `main` |
| Migrations (`supabase/migrations/**`) | GitHub Action `deploy-supabase.yml` |
| Functions (`supabase/functions/**`) | GitHub Action `deploy-functions.yml` |
| Super admin / tenants | Platform UI or `seed:platform-admin` (manual) |
| Lookups / counters after wipe | `npm run seed:reference-data` |
| Full wipe local/prod | `npm run db:wipe -- --local` or `--prod` |

## Local development

See [README.md](README.md). Edge functions require `npm run functions:serve` in a second terminal alongside `npm run dev`.
