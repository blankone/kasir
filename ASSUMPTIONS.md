# LaundryKu — Assumptions

- Auth: Better Auth feasible; jika plugin Organization tidak mendukung role custom, implement Membership sendiri (fallback).
- Region Vercel: sin1. DB Neon region Singapore.
- Pembayaran gateway F9: iPaymu (sandbox dulu), bukan Midtrans.
- Auth: Better Auth — cek docs versi terbaru sebelum implement (Context7).">
- Rate limit opsional: jika Upstash tidak dikonfigurasi, no-op limiter (log warning) agar dev tetap jalan.
- PIN kasir: hash dengan bcrypt/scrypt, panjang 4-6 digit, lockout setelah 5 gagal.
- Cetak nota: window.print dengan CSS @media print 58/80mm; tidak ada server-side PDF di F1.
- Pajak default 0%, rounding nearest 100 IDR, dapat diubah per tenant.
- Trial 14 hari dari registrasi; plan default "TRIAL".
- Slug reserved: admin, api, www, app, platform, marketing, auth, static, assets, login, register.
- Super admin dibuat via seed/env SUPER_ADMIN_EMAIL.
- Timezone tampil Asia/Jakarta; DB UTC.
- Deposit top-up dicatat sebagai CashMovement IN kategori "DEPOSIT".
- QR label: encode order code; render client-side (qrcode lib).
