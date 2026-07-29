# AlgoLend — Bespoke Digital Credit & Risk Management Platform

## Project overview

AlgoLend is a full-stack Node.js/Express loan-origination platform for Zwane Financial Services. It supports a multi-step borrower application flow (Documents → Credit Check → Loan Selection → Confirmation), an admin portal, KYC verification, credit-bureau integration, DocuSeal contract signing, push notifications, and a comprehensive audit trail.

**Stack:** Node.js 20, Express, Supabase (PostgreSQL + Auth), vanilla JS frontend (no bundler on the user-portal side), Vite + React for the admin portal.

## How to run

Dependencies are installed with `npm install` (done). The workflow `Start application` runs `node server.js` and serves on port 5000.

```
node server.js
```

## Required secrets

The following environment secrets must be set before the app is fully functional:

| Secret | Purpose |
|---|---|
| `SUPABASE_ANON_KEY` | Supabase public key (also set as `VITE_SUPABASE_ANON_KEY`) |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase service-role key for server-side operations |
| `SESSION_SECRET` | Express session signing (already configured) |

Optional secrets (integrations will fail gracefully without them):
- `EXPERIAN_USERNAME` / `EXPERIAN_PASSWORD` — credit-bureau checks
- `RESEND_API_KEY` — transactional email
- `VAPID_PUBLIC_KEY` / `VAPID_PRIVATE_KEY` — push notifications
- `DOCUSEAL_API_TOKEN` — contract signing
- `DIDIT_API_KEY` — KYC (DiDiT / TruID)

`SUPABASE_URL` is already configured in `.replit` shared env vars.

## Demo mode

Add `?demo=true` to any user-portal URL to activate demo mode. This persists in `sessionStorage` and bypasses:
- Session/auth guard (no login required)
- Document upload and KYC requirements (Step 1)
- Credit-check requirement (Step 2 & 3)
- Pre-fills a sample loan config on the Confirmation page (Step 4)

Example: `https://<your-repl>.repl.co/user-portal/?demo=true`

To exit demo mode: open the browser console and run `sessionStorage.removeItem('demoMode')`, then refresh.

## Key directories

| Path | Contents |
|---|---|
| `server.js` | Main Express server (all API routes) |
| `public/user-portal/` | Borrower-facing SPA (vanilla JS) |
| `public/admin/` | Admin portal (Vite + React) |
| `public/auth/` | Login / password-reset pages |
| `public/shared/` | Shared assets, theme runtime, loader |
| `services/` | Server-side service modules |
| `config/supabaseServer.js` | Supabase server client |
| `migrations/` | SQL migration files |
| `sql/` | Schema and audit-trail SQL |

## User preferences

- Keep existing project structure — do not migrate or restructure without asking.
- Use the AlgoLend logo (`/shared/algolend-logo.png`) as the default brand logo fallback.
