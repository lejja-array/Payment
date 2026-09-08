# PaymentVerify Ethiopia — Phase 6

## Dashboard & Analytics

Phase 6 adds role-scoped payment analytics to `/dashboard` and expands `/api/dashboard` with payment metrics, daily volume, provider breakdown, branch performance, and recent payments.

### Security
- SUPER_ADMIN sees platform-wide data and may filter by organization/branch.
- ORGANIZATION_ADMIN sees only their organization.
- BRANCH_MANAGER, AGENT and VIEWER see only payments belonging to their assigned branches.
- Scope is enforced server-side; the dashboard UI is not the security boundary.

### Run
npm install
npm run prisma:generate
npm run prisma:migrate
npm run dev