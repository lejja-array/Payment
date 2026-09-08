# PaymentVerify Ethiopia — Phase 3: QR Code Management

## Implemented
- QRCode Prisma entity with organization, optional branch, payment account, payload, type, status, expiry and audit ownership.
- Strict organization/branch/payment-account consistency checks.
- Active QR codes require an active organization, active payment account and active branch when branch-scoped.
- QR identifier uniqueness per organization.
- QR management API: list, create, read, update.
- Role-aware QR visibility: branch-level users see only QR codes assigned to their authorized branches.
- QR payload preview using `qrcode.react`.
- Create/edit/activate/deactivate/preview UI.
- Account-number masking for non-admin users.
- Explicit security boundary: QR scanning is identification, not payment verification.
- Prisma/db export compatibility and assigned-branch helper added to stabilize Phase 2 integration.

## Database
Run:

```bash
npm install
npm run prisma:generate
npm run prisma:migrate -- --name phase3_qr_codes
```

Do not mark payments verified from QR data. Payment verification is a later phase.

## Phase 3 acceptance checks
1. Two organizations cannot see each other's QR codes.
2. Branch Manager/Agent/Viewer only see QR codes for assigned branches.
3. A QR cannot point to a payment account in another organization.
4. A branch QR cannot point to a different branch-specific payment account.
5. Active QR codes cannot be created for inactive organization/branch/account.
6. Duplicate QR identifiers are rejected within an organization.
7. QR activation/deactivation is audited.
8. QR preview encodes only the stored payload and never claims payment verification.

## Remaining
Phase 4 will implement QR Payment Capture: camera scanning, image upload, manual fallback, payment record creation, duplicate detection and capture audit trail.