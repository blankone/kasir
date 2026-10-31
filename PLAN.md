# LaundryKu — Plan

## Arsitektur
- Next.js App Router + TS strict, RSC + Server Actions. Tailwind + shadcn/ui + lucide-react.
- Multi-tenant shared DB/shared schema, `tenantId` pada semua tabel bisnis, indexed.
- Tenant dari session (slug hanya branding). Helper `withTenant(ctx)` + Prisma extension guard + Server Action guard.
- Auth: Better Auth (email+password) + Organization plugin. Alternatif: Lucia/NextAuth jika Better Auth plugin bermasalah — keputusan di F1 setelah cek docs.
- DB: Neon (pooled DATABASE_URL runtime, DIRECT_URL migrate) + Prisma driver adapter serverless.
- Upload: Vercel Blob. Email: Resend. Rate limit: Upstash Redis abstraction (no-op fallback).
- Realtime: polling (kanban 15s, POS refresh interval). Tanpa WebSocket.
- Uang INTEGER rupiah; berat Decimal(10,2); waktu UTC tampil Asia/Jakarta.
- Invoice: `Sequence` per tenant+outlet+key+period, generate dalam transaksi DB, format `INV/JKT01/2026-10/0001`.

## ERD (teks)
Tenant 1..* Outlet, User 1..* Membership (*..1 Tenant, ? Outlet), Invitation.
Tenant 1..* Customer/Service/Order/Expense/Shift/Sequence/AuditLog.
Order 1..* OrderItem (*..1 Service), 1..* Payment (*..1 Shift). Shift 1..* CashMovement.
Soft-delete: Order, Customer, Service.

## Fase
F1 Fondasi: init, schema+migrasi+seed, auth+tenant context, layout, deploy Vercel.
F2 Pendaftaran/onboarding, outlet/staf/role, platform panel.
F3 Master data layanan & pelanggan + pengaturan tenant.
F4 POS + pembayaran + invoice + cetak nota/label.
F5 Shift multi-kasir + kas in/out + laporan shift.
F6 Order lifecycle + papan produksi + lacak publik + notif WA/email.
F7 Dashboard & laporan + export + audit log viewer.
F8 Hardening: test isolasi & kalkulasi, rate limit, error boundary, empty states, polish, docs, optimasi.
F9 (opsional) billing Midtrans, stok, pickup-delivery, PWA offline.

## Risiko
- Better Auth Organization plugin API berubah → cek docs Context7 sebelum implement.
- Prisma serverless adapter + migrate deploy di Vercel build.
- Race invoice number → pakai transaksi + Sequence row lock.
- IDOR → 3 lapis guard + test otomatis.
- Cron Hobby 1x/hari → jangan andalkan real-time.
- Filesystem read-only → Vercel Blob wajib.

## Struktur folder
src/app/(marketing)/ | (auth)/ | (app)/[tenantSlug]/(dashboard|pos|orders|customers|reports|settings)/ | (platform)/platform/ | api/
src/server/{auth,db,tenant,repos,services,actions}/
src/components/{ui,pos,orders,reports}/
src/lib/{money,datetime,invoice,print,validators}/
prisma/{schema.prisma,migrations,seed.ts} tests/ docs/
