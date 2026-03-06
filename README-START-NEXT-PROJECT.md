# Start Your Next Project (After Cloning)

When you clone this SaaS template and start a new product, use this as a reminder of **what to enable** and **what tools** the stack uses.

---

## Tools in This Stack

| Layer        | Technology   | Purpose |
|-------------|--------------|---------|
| **Frontend** | Next.js      | App Router, React, Tailwind, Shadcn UI |
| **Backend**  | Express (Node)| REST API, auth, billing, webhooks |
| **Database** | Supabase     | Auth, users, organizations, subscriptions, data |
| **Email**    | Resend       | Password reset, notifications (optional) |
| **Billing**  | Stripe       | Checkout, subscriptions, webhooks |

All of these are **disconnected by default**. You add real keys and URLs when you’re ready. See **Enable services** below.

---

## 1. Install & env (do this first)

From the repo root:

```bash
npm install
npm run install:all
```

- **Backend env:** Copy `backend/env.example` → `backend/.env.development` (and `.env.production` when you deploy).
- **Frontend env:** Copy `frontend/env.example` → `frontend/.env.development` (and `.env.production` for production builds).

You can leave placeholder values to run locally; the app will start. Auth, billing, and email only work after you add real keys.

---

## 2. Dev mode: run frontend and backend

| Command | What runs | URL |
|--------|-----------|-----|
| `npm run dev` or `npm run dev:all` | Backend + frontend together | Frontend: **http://localhost:3000**, Backend: **http://localhost:3002** |
| `npm run dev:frontend` | Frontend only | **http://localhost:3000** |
| `npm run dev:backend` | Backend only | **http://localhost:3002** |

- **Frontend dev:** Next.js dev server (hot reload).
- **Backend dev:** Express with `node --watch` (restarts on file change).

For local UI work without logging in, set in `frontend/.env.development`:

```bash
NEXT_PUBLIC_BYPASS_AUTH=1
```

(Only in development; never in production.)

---

## 3. Enable services when ready

| Service   | What to enable | Where |
|----------|----------------|-------|
| **Database (Supabase)** | Create a project at [supabase.com](https://supabase.com), run migrations (`backend/schema.sql`, `backend/migration-*.sql`), then set `SUPABASE_URL`, `SUPABASE_ANON_KEY`, and optionally `SUPABASE_SERVICE_ROLE_KEY` in `backend/.env.development` (and production). | `backend/env.example`, `backend/lib/supabase.js` |
| **Email (Resend)** | Sign up at [resend.com](https://resend.com), get an API key. Set `RESEND_API_KEY` (and optionally `RESEND_FROM_EMAIL`, `ADMIN_EMAIL`) in backend env. Until then, the app runs but does not send email. | `backend/lib/email.js`, `backend/env.example` |
| **Billing (Stripe)** | Create a Stripe account and products/prices. In **backend** env set `STRIPE_SECRET_KEY` and (for webhooks) `STRIPE_WEBHOOK_SECRET`. In **frontend** env set `NEXT_PUBLIC_STRIPE_PRODUCT_ID`, `NEXT_PUBLIC_STRIPE_MONTHLY_PRICE_ID`, `NEXT_PUBLIC_STRIPE_ANNUAL_PRICE_ID`. Use test keys/IDs in dev; switch to live in production. | `backend/routes/billing.js`, `backend/routes/auth.js`, `frontend/lib/billing.ts`, frontend env |
| **Deploy / API URL** | Deploy backend (e.g. Railway) and frontend (e.g. Vercel). Set `NEXT_PUBLIC_API_URL` in frontend to your deployed backend URL (e.g. `https://your-app.up.railway.app`). | `frontend/env.example` |

---

## 4. Quick reference

- **Checklist (step-by-step):** **CHECKLIST-SAAS-BOILERPLATE.md** — how to connect each service.
- **Task history / reapply:** **README-Tasks-To-Reapply.md** — what was boilerplated and how the template was simplified.
- **Env examples:** `backend/env.example`, `frontend/env.example` — all variables and comments.

---

## Summary

1. **Clone** → `npm install` + `npm run install:all`.
2. **Copy** backend and frontend env examples to `.env.development`.
3. **Run** `npm run dev` (or `npm run dev:frontend` / `npm run dev:backend`) for dev mode.
4. **Enable** Supabase, Resend, and Stripe when you need auth, email, and billing.
5. **Deploy** backend and frontend, then set `NEXT_PUBLIC_API_URL` in the frontend.
