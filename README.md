# PaymentVerify Ethiopia — Phase 1 (Backend + Auth)

Phase 1 now includes working HTTP-only cookie authentication, organization/branch APIs, branch assignment APIs, server-side RBAC checks, and audit logging.

## Stack
Next.js + TypeScript, PostgreSQL + Prisma, bcryptjs, jose, Zod.

## Setup
1. Copy `.env.example` to `.env` and configure PostgreSQL.
2. `npm install`
3. `npx prisma generate`
4. `npx prisma migrate dev --name phase1_foundation_auth_org_branch`
5. `npm run dev`

## Authentication
- `POST /api/auth/login` — creates an HTTP-only 8-hour session cookie.
- `POST /api/auth/logout` — clears the session cookie.
- `GET /api/auth/me` — returns the current authenticated user.

## Organization API
- `GET /api/organizations`
- `POST /api/organizations` — SUPER_ADMIN only.
- `GET /api/organizations/:id`
- `PATCH /api/organizations/:id` — SUPER_ADMIN or organization admin for own organization.

## Branch API
- `GET /api/branches?organizationId=...`
- `POST /api/branches` — SUPER_ADMIN or ORGANIZATION_ADMIN for the target organization.
- `GET /api/branches/:id`
- `PATCH /api/branches/:id`

## User/Branch assignment
- `GET /api/users/:id/branches`
- `POST /api/users/:id/branches` with `{ "branchIds": ["..."] }` — SUPER_ADMIN or ORGANIZATION_ADMIN.

## Security
Authorization is checked on the server. Organization admins cannot access another organization, branch managers/agents are not granted organization-admin privileges, and branch manager assignment is validated against the target organization. Audit records are written for login and important organization/branch/assignment changes.

Do not put bank/wallet provider secrets in client code. QR scanning must never be treated as payment verification.

## Phase 1 remaining
Production-grade UI, user creation/invitation endpoints, role-specific dashboards, automated integration/security tests, rate limiting, CSRF strategy as needed by deployment architecture, and hardened session/token rotation. Only after these are tested should Phase 2 begin.

## Phase 1 — User Management & Dashboard

Added in the current increment:
- User listing and creation API with role restrictions.
- User detail/update API with password hashing.
- User-to-branch assignment API.
- Platform/organization dashboard metrics.
- Protected dashboard page.
- Audit logging for user creation/update and branch assignment.

Before production deployment, set a strong `JWT_SECRET`, configure PostgreSQL, run Prisma migrations, and add automated integration/security tests.


## Phase 1 hardening update

Branch-level visibility is now enforced in branch list/detail and dashboard queries. Organization administrators can access their organization management page, while organization creation remains SUPER_ADMIN-only. Production JWT configuration now requires `JWT_SECRET`. A regression checklist is included at `tests/phase1-security-checklist.md`.

**Before production:** run `npm install`, `npx prisma generate`, configure PostgreSQL and `JWT_SECRET`, run `npx prisma migrate dev` in development (or the appropriate deployment migration command), then run `npm run build`.


## Phase 2 Provider Configuration
- Organization-scoped PaymentProviderConfig controls which providers are enabled.
- Active payment accounts require an enabled provider configuration.
- Provider names/codes come from the controlled provider registry.
- No bank/wallet credentials or fake API verification are stored in the frontend.

## Phase 10 — Final Integration & Release Candidate

This release adds authorization-aware global search (`/api/search`) and a global search box in the authenticated header. Search covers organizations, branches, payments, payment accounts, QR codes, and users where the current role is authorized to see them. Sensitive payment-account numbers are masked in search results.

Before release, run the complete validation sequence in `tests/phase10-release-checklist.md`. A package lockfile must be generated in a network-enabled environment before using the repository's `npm ci` CI workflow.