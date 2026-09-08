# PaymentVerify Ethiopia — Phase 5: Payment Verification

## Implemented
- Verification queue with organization/branch scoping.
- Explicit VERIFIED / REJECTED decisions.
- Authorized verifiers: SUPER_ADMIN, ORGANIZATION_ADMIN, BRANCH_MANAGER.
- Branch managers are limited to assigned branches.
- Capturer cannot verify their own payment.
- Rejection requires a reason.
- VerificationRecord history records verifier, decision, reason, notes and timestamp.
- Verified/rejected payments cannot be silently changed through this endpoint.
- Audit events for verification and rejection.
- QR scanning is never treated as proof of payment.

## Database
Added `VerificationDecision` enum and `VerificationRecord` model.

Run:
```bash
npm install
npm run prisma:generate
npm run prisma:migrate -- --name phase5_payment_verification
npm run dev
```

## Important production note
This phase provides manual verification controls. It does not claim to connect to Ethiopian banks/wallets. A future provider adapter layer can add real API verification when credentials, contracts and provider APIs are available.