## Tasks to Reapply After Reset

This file summarizes the edits we made during the last session so they can be systematically re‑applied if needed. Treat these as a checklist.

### 1. Docs, repo, and env setup

- [x] **Docs cleanup and template context**
  - [x] Update `README.md` to describe this as a generic SaaS Template rather than the original Clermont product.
  - [x] Update `README-DEV-PROD-SETUP.md` with:
    - [x] How to run frontend and backend locally.
    - [x] How to connect or *not* connect Supabase, Stripe, Resend, Railway (placeholders are fine).
  - [x] Update `README-Repo-and-Commits.md` to:
    - [x] Describe the new repo history.
    - [x] Document the expected commit style and branching.
  - [x] Remove/ignore Clermont‑specific docs and migration READMEs that don’t apply to the template.

- [x] **Env files**
  - [x] Ensure `frontend/.env.development`:
    - [x] Uses placeholder values for API URL, Stripe IDs, Google Maps key, etc.
    - [x] Includes `NEXT_PUBLIC_BYPASS_AUTH=1` for local dev.
  - [x] Ensure `frontend/env.example` documents all required `NEXT_PUBLIC_*` vars and the auth bypass flag.

### 2. Auth bypass for local development

- [x] **Workspace layout (`frontend/app/(workspace)/layout.tsx`)**
  - [x] Add a development‑only bypass before any token checks:
    - [x] Compute `isDevBypassAuth` as:
      - [x] `process.env.NODE_ENV === "development"` and
      - [x] `process.env.NEXT_PUBLIC_BYPASS_AUTH === "1"`.
    - [x] When `isDevBypassAuth` is true:
      - [x] Set `userData` to a dummy user/organization (e.g. “Dev User”, “Dev Organization”).
      - [x] Set `isAuthenticated` to `true` and `isChecking` to `false`.
      - [x] `return` early from `checkAuth`.
  - [x] Keep the existing production path:
    - [x] If no token: redirect to `/login`.
    - [x] If token invalid: clear token, redirect to `/login`.
    - [x] Otherwise: call `getCurrentUser()` and populate `userData`.

### 3. Landing / marketing site boilerplate

- [x] **Global layout & styling**
  - [x] In `frontend/app/layout.tsx` and shared landing components:
    - [x] Replace “Clermont”/product‑specific copy with boilerplate:
      - [x] Hero: “Accomplish anything with Company Name” and a generic supporting sentence.
      - [x] CTA button text like “Try For Free”.
    - [x] Update pricing anchor link to use `#pricing` and ensure scrolling is smooth (e.g. `scroll-behavior: smooth` in `globals.css`).

- [x] **Sections & components**
  - [x] Replace feature/benefit copy with generic values:
    - [x] Use placeholders like “Value Proposition 1/2/3” and “VP Description 1/2/3”.
  - [x] Replace pricing card feature lists with generic “Feature 1/2/3”.
  - [x] Replace FAQ questions/answers with:
    - [x] `FAQ Question 1–5` and `FAQ Answer 1–5` plus short placeholder descriptions.
  - [x] Update footer:
    - [x] Company name → `Company Name`.
    - [x] Tagline → `Tagline or short description for your product.`.

### 4. Address input simplification

- [x] **`frontend/components/address-autocomplete.tsx`**
  - [x] Ensure this is a plain text input (no Google Maps integration):
    - [x] On blur / Enter, call `onAddressSelect` with a minimal payload:
      - [x] `formatted_address` as the typed value.
      - [x] Empty or placeholder fields for anything map‑specific.
  - [x] Remove or neutralize any New York–specific borough checks or zoning hints.

### 5. Workspace route & page simplifications

- [x] **Routing changes**
  - [x] Remove the old `report-options` route.
  - [x] Add an `information-gather` route and wire any links that previously pointed to `report-options` to `/information-gather`.
  - [x] Remove the old `signupsearch` route.
  - [x] Add a `signupbytrying` route and wire the flow:
    - [x] `/information-gather` → Continue → `/signupbytrying`.

- [x] **Information gather page (`frontend/app/information-gather/page.tsx`)**
  - [x] Use neutral boilerplate:
    - [x] Heading: “Page Description”.
    - [x] Three choices labeled `Option 1`, `Option 2`, `Option 3`.
    - [x] A primary button that continues to `/signupbytrying`.

- [x] **Signup by trying page (`frontend/app/signupbytrying/page.tsx`)**
  - [x] Create a signup form with:
    - [x] Email, password, and confirm password fields.
    - [x] Eye/visibility toggle icons on password and confirm‑password fields.
    - [x] Validation that password and confirm password match (with a user‑visible error).

- [x] **Simplify workspace content pages**
  - [x] `frontend/app/(workspace)/home/page.tsx`
  - [x] `frontend/app/(workspace)/main-page-1/page.tsx` (replaces `search-address`)
  - [x] `frontend/app/(workspace)/reports/page.tsx`
  - [x] For each, replace complex Clermont UI with:
    - [x] A simple heading explaining that this is a placeholder.
    - [x] Short TODO comments or guidance indicating:
      - [x] Add proper auth token handling.
      - [x] Fetch real data from backend/DB.
      - [x] Replace view using Shadcn UI components (tables, filters, etc.).

- [x] **Remove Land Assemblage workspace page**
  - [x] Delete `frontend/app/(workspace)/land-assemblage/page.tsx`.
  - [x] Remove the “Land Assemblage” item from:
    - [x] The workspace sidebar.
    - [x] Any `getPageTitle` logic that returns “Land Assemblage”.

### 6. Demo dashboard list page (`demo-report-list`)

- [x] **Route & layout**
  - [x] Ensure `/demo-report-list` is accessible from the workspace sidebar.
  - [x] Update `getPageTitle` in `(workspace)/layout.tsx` so:
    - [x] `/demo-report-list` → “Sample Dashboard” (or similar neutral title).

- [x] **Boilerplate metrics**
  - [x] In `frontend/app/(workspace)/demo-report-list/page.tsx`:
    - [x] Change the greeting section to a neutral heading like `Sample Dashboard` instead of “Hi Elie”.
    - [x] Replace “Report Dashboard” and “Insights” with a single “Metrics” section containing three cards:
      - [x] `Metric 1`, `Metric 2`, `Metric 3`.
      - [x] Each shows a placeholder value (e.g. `—`) and short helper text like “Placeholder value”.

- [x] **Items table section**
  - [x] Section title: change `Sample Reports` → `Items`.
  - [x] Table column headings:
    - [x] `Address` → `List`.
    - [x] Remove the `Zoning` column entirely (header + cell).
  - [x] Dummy data:
    - [x] Use boilerplate items and people:
      - [x] `Item 1`, `Item 2`, `Item 3`, … for the `address`/list field.
      - [x] `Person 1`, `Person 2`, `Person 3`, … for the `clientName`.
    - [x] Keep `status`, `createdAt`, and the `View Report` button behavior.
  - [x] Clean up icon imports:
    - [x] Remove any unused icons like `MapPin`, `Building2`, `Sparkles` if no longer referenced.

### 7. Demo detail report page (`demo-report/[id]`)

- [x] **Header & navigation**
  - [x] In `frontend/app/(workspace)/demo-report/[id]/page.tsx`:
    - [x] Change the main title from “Property Zoning Report” → `Sample Item Report`.
    - [x] Change the back button text from “Back to Your Reports” → `Back to Items`, keeping the same route (`/demo-report-list`).
    - [x] Keep existing icons and the share button behavior.

- [x] **Boilerplate data object**
  - [x] Replace the `propertyData` object with fully generic values:
    - [x] `address` → `Item 1`.
    - [x] All nested fields (`lotDetails`, `zoning`, `zoningDetails`, `buildingInfo`, `landUse`, etc.) use neutral placeholders:
      - [x] `Sample value 1/2/3…`
      - [x] `Sample classification`, `Sample designation`, etc.
      - [x] Short boilerplate descriptions instead of zoning jargon.
    - [x] `allowedUses`, `restrictedUses`, and `feasibleOptions`:
      - [x] Rename entries to `Example allowed use X`, `Example restricted use X`, and `Scenario 1/2/3` with short neutral descriptions and considerations.

- [x] **Section titles and labels**
  - [x] Rename cards:
    - [x] “Property Location” → `Item Overview`.
    - [x] “Lot Details” → `Item Details`.
    - [x] “Zoning Classification” → `Section A`.
    - [x] “Building Lot Information” → `Section B`.
    - [x] “Land Use Designation” → `Section C`.
    - [x] “Zoning Constraints & Requirements” → `Section D`.
    - [x] “Allowed Uses” → `Highlights`.
    - [x] “Restricted Uses” → `Limitations`.
    - [x] “Feasible Development Options” → `Scenarios`.
  - [x] For Sections B, C, and D, change field‑level labels to boilerplate:
    - [x] Use generic labels instead of domain-specific terms.
    - [x] Where a group of fields exists, use generic sub-labels (e.g. `Sub-metric 1/2/3`).
    - [x] Helper paragraphs are generic, e.g. “Sample description for this field.”

### 8. Login page & auth UI polish

- [x] **Login page (`frontend/app/login/page.tsx`)**
  - [x] Update heading/copy to generic wording:
    - [x] Title: `Login`.
    - [x] Generic subtext about accessing your account. (Handled within the page content/flow.)
  - [x] Keep layout simple and neutral; logo placement can be adjusted later if desired.

---

Once you start reapplying these tasks, it’s a good idea to:

1. Work through this list top‑to‑bottom and check off items as you go.
2. Create a commit after each logical group (e.g. “Boilerplate demo dashboard”, “Boilerplate demo detail report”, “Auth bypass for dev”) so this work is saved in git history.

