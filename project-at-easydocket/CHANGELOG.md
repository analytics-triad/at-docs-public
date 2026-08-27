# Changelog

All notable changes to DocketFlow are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Version history lives in this file in the repo. Optionally mirror releases on
GitHub Releases when you want downloadable notes per tag.

## [Unreleased]

### Added

### Changed

### Fixed

## [0.3.11] — 2026-08-27

### Added

- GitHub Actions workflow to mirror root markdown docs to `analytics-triad/at-docs-public/project-at-easydocket` on push to `main`.


## [0.3.10] — 2026-08-19

### Fixed

- Platform console PATCH/POST calls (save tenant, email toggle, create tenant admin) failed on mobile/Safari with “Network error” — add anon `apikey` header to platform API fetch and CORS `Allow-Methods` on platform Edge Functions.


## [0.3.9] — 2026-08-19

### Added

- **Email sharing** — Share submitted dockets by email with PDF attachment; async send via `send-email` Edge Function (Resend adapter), `email_log` audit trail, and retry policy.
- **Email Settings & template** — Tenant admin configures To/CC recipients (including Other Recipients), and edits the docket sharing HTML template with placeholders.
- **Offline email queue** — Docket share intents queue in IndexedDB when offline and send when back online.
- **Notifications** — Bell icon, toast alerts, failed-email banner, and Notifications page for email send status.
- **Time Zone & Locale** — Tenant admin sets timezone, date/time format, and number/currency locale; applied across the app.
- **Per-tenant email toggle (platform super admin)** — Enable/disable email per tenant at `/platform/tenants/:id`; disables Share, Email Settings, notifications, and cancels queued/sending emails.
- **Per-tenant email provider config** — Super admin configures Resend credentials and sender identity per tenant in the platform console.
- **Login page redesign** — Split-panel layout with branding panel.

### Changed

- Docket PDF generation extended for email sharing (storage upload to private `docket-pdfs` bucket).
- CC recipient resolution includes contract administrator, site manager, org users, and other emails; parses `Name <email>` formats.
- Platform `platform-tenant` Edge Function now updates `email_enabled` (service role) so the toggle persists under RLS.

### Fixed

- Email feature toggle no longer silently reverts on reload (was blocked by tenant RLS; now routed through platform API).


## [0.3.8] — 2026-08-19

### Added

- Client Sign Off collects Representative Name and Email (required) and Phone (optional) on the docket; shown on Detail and PDF. Invalid Email shows a red border and **Email is not valid.** Signature pad shows grey **Signature** until signed; **Clear Signature** only after ink.

### Changed

- New Docket heading no longer shows **Pending sync** / **Queued** pills (those remain on the Dockets list and Docket Detail).
- Docket line **Add Notes** uses a note document with a plus when empty and the lined note icon after a note is saved.
- **Visit Comments** renamed to **Docket Comments** (create, detail, and PDF); create-form label matches other input labels.
- **+ Add Plan** matches **+ Add Variation Item**; plan editor uses icon-only Save and Cancel on the input row.
- **+ Add Variation Item** is hidden while the Variation editor is open (returns after Save or Discard), matching **+ Add Plan**.
- **Total Hours Today** shows **N/A** (grey) instead of **—** when there are no hours (create, detail, and PDF).

### Fixed


## [0.3.7] — 2026-08-18

### Added

### Changed

### Fixed

- Contract Contacts unit tests mock Supabase so CI can run without env vars.


## [0.3.6] — 2026-08-17

### Added

### Changed

- Submitted Dockets filters collapse by default behind a **Filters** toggle; active filters show as dismissible chips, with **Reset** on the toolbar. Deep link `?variationStatus=` opens the panel.
- Default **Last month** start-date window shows as a chip; clearing it shows all submitted dockets; **Reset** restores defaults.

### Fixed


## [0.3.5] — 2026-08-17

### Added

- Contract Contacts on New Contract and Contract Details: optional Contract Administrator and Site Manager (name, phone, email) in `contract_contacts`, between Site address and Contract date.

### Changed

- System Contract No is no longer shown on New Contract or Contract Details (still allocated in the database; lists and forms use Contract No only).
- Contract form: **Project name** → **Contract Name**; identity row then a **Client** section (**Client Name**, **Billing type**). Lists, dockets, View Progress, and PDF use **Contract Name**.

### Fixed


## [0.3.4] — 2026-08-13

### Added

### Changed

### Fixed

- PDF readiness unit tests no longer import offline sync/Supabase (fixes CI `supabaseUrl is required`).


## [0.3.3] — 2026-08-13

### Added

- View PDF for submitted dockets (A4, on-demand in the browser; not stored): Detail button and Dockets list icon when complete; firm header from Organizational Profile.
- Pending sync badge alongside Queued / Sync Error on create, detail, and docket list.

### Changed

- After Submit, open the new docket’s detail (Back returns to create origin). Save as Draft returns to create origin.
- Docket Detail View PDF stays disabled with Offline until number, sync, and signature are ready; detail refreshes in place after sync.

### Fixed


## [0.3.2] — 2026-08-12

### Added

### Changed

- Require a patch version bump on every production commit (Cursor rule). Documented in DEPLOY.md.

### Fixed

- Docket Lines variation banner no longer treats **At Limit** (completed-with-today equals agreed quantity) as variation work beyond the agreement. Select step still warns when an item is already at or over the limit from prior submitted progress.


## [0.3.1] — 2026-07-15

- Patch release (see git history)


## [0.3.0] — 2026-07-15

### Added

- Schedule of Rates migrations and job SoR UI (categories, billing methods, agreed/variation rates)
- Works Records flow: draft/submitted statuses, offline sync badges, create/detail polish (lines, plans, signature)
- Work Analysis page with Contract and Variations SoR grids after client/job select
- Docket plans on create/view; database wipe script and deploy/seed docs

### Changed

- Nav: Works Records, Work Analysis, Home; Parameters under Settings; Progress Claims removed from primary nav
- Completed qty on SoR progress is sum of submitted line `qty`; variation_qty / is_variation computed and validated on save

### Fixed

- Variation line math: full user qty saved; excess stored as variation_qty; UI/DB mismatch rejects save


## [0.2.0] — 2026-07-14

### Added

- Client contacts table with multiple contacts per client and a selectable primary contact
- Client Xero Contact ID and Active/Inactive status (reference only for now)
- Jobs workflow (Client → Job): job numbering, billing types, job statuses; Projects removed

### Changed

- Client management (and contacts) restricted to tenant administrator; supervisors keep project/contract access
- Removed client-level phone/email/contact person fields in favour of `client_contacts`
- UI rename Contracts → Jobs; only tenant admin creates/edits jobs; supervisors assign technicians and view non-draft/non-archived jobs

### Fixed


## [0.1.0] — 2026-07-14

### Added

- Platform super-admin area and tenant lifecycle management
- Tenant profile, docket numbering, and settings
- Roles table with Supervisor, Field Technician, View Only (Administrator assignable later)
- Create user (password or invite), employee number, profile self-service
- Immediate access revocation on user deactivate (sessions + RLS; data retained)
- In-app version label (`vX.Y.Z · gitSha`) in the left navigation

### Changed

- Renamed Project Manager → Supervisor and Field Worker → Field Technician
