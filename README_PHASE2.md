# Phase 2 — Payment Infrastructure

This increment adds organization/branch-scoped Payment Accounts, provider metadata, CRUD APIs, audit events, and the management UI.

## Database
Run:
`npx prisma generate`
`npx prisma migrate dev --name phase2_payment_accounts`

## Security
Payment accounts are server-side scoped by organization. Branch-level users can only read accounts belonging to assigned branches. Only SUPER_ADMIN and ORGANIZATION_ADMIN can create/update accounts. Account number/provider uniqueness is enforced per organization.

QR scanning is not payment verification. No live bank/wallet API is claimed or simulated.