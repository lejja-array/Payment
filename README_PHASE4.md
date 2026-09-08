# PaymentVerify Ethiopia — Phase 4

## QR Payment Capture

Implemented camera QR scanning, QR image upload, manual fallback, payment capture, receipt upload, duplicate transaction detection, authorization, and audit logging.

### Security boundary
QR scanning identifies payment instructions only. It never marks a payment verified. Verification is a separate Phase 5 workflow.

### Receipt storage
Receipts are stored outside `public/` and served only through an authenticated, organization/branch-authorized endpoint. The included filesystem storage is intended for development; production should replace it with private object storage through a storage adapter.

### Database
Run `npm install`, `npm run prisma:generate`, then `npm run prisma:migrate -- --name phase4_payments`.