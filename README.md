# PotholeMap

Peta laporan jalan berlubang dari masyarakat. Pengguna nantinya dapat mengirim satu foto dan lokasi tanpa akun; admin meninjau laporan sebelum ditampilkan ke publik.

**Status:** Fondasi proyek sedang disiapkan di M0. Aplikasi, API, database, dan deployment belum diimplementasikan atau diverifikasi. Belum ada public beta atau kanal pengiriman laporan aktif.

PotholeMap membantu visibilitas kondisi jalan. Ini bukan kanal pengaduan resmi pemerintah dan publikasi laporan tidak menjamin perbaikan.

## Rencana MVP

- Peta responsif dengan laporan yang sudah disetujui, filter status, daftar dan detail.
- Pelaporan anonim dengan satu foto, GPS opsional atau pin lokasi manual.
- Foto diproses secara aman dan disimpan privat sebelum publikasi yang terkontrol.
- Login admin, MFA, moderasi dan audit perubahan status.
- Proteksi spam, privasi, monitoring kuota, backup/restore dan rollback.

AI, akun publik, voting, notifikasi, integrasi pemerintah, microservices, Redis dan message broker berada di luar baseline MVP.

## Stack yang direncanakan

| Bagian | Baseline |
|---|---|
| Frontend | React + Vite + TypeScript strict |
| Backend/hosting | Cloudflare Workers Free, satu API modular monolith |
| Data | Supabase PostgreSQL + PostGIS |
| Foto dan admin | Supabase private Storage + Auth/MFA |
| Peta dan anti-bot | Leaflet + tile provider yang akan dipilih; Cloudflare Turnstile |
| Tooling | pnpm, Vitest, ESLint, Prettier, GitHub Actions dan Wrangler |

Kelayakan runtime, pemrosesan foto dan batas free tier harus dibuktikan sesuai milestone. Target biaya awal **Rp0** bukan jaminan biaya operasional nol selamanya atau uptime tertentu. Tidak ada upgrade berbayar otomatis tanpa persetujuan maintainer.

## Mulai dari dokumentasi

1. Baca [master plan](docs/PROJECT_MASTER_PLAN.md) untuk scope dan arsitektur.
2. Buka [indeks milestone](docs/milestones/README.md) untuk urutan M0–M13.
3. Ikuti [kontrak M0](docs/milestones/M0_ENGINEERING_FOUNDATION.md) dan satu issue yang dipilih.
4. Baca [panduan kontribusi](CONTRIBUTING.md); agent juga wajib membaca [AGENTS.md](AGENTS.md).

Belum ada `package.json`, perintah menjalankan aplikasi, test runner atau pipeline CI di tahap governance ini. Bootstrap/tooling diperkenalkan melalui issue M0 berikutnya; jangan menjalankan scaffold seluruh aplikasi untuk menyelesaikan dokumen.

## Kontribusi dan security

- Satu issue → satu branch → satu PR terfokus → verifikasi → review → squash merge.
- Proposal atau draft milestone tidak otomatis mengizinkan implementasi semua fitur.
- Bug biasa dapat dilaporkan melalui [issue](https://github.com/LaurensiusDani/PotholesMap/issues/new/choose), tanpa data pribadi atau kredensial.
- Untuk celah keamanan, ikuti [SECURITY.md](SECURITY.md) dan gunakan kanal privat.
- Status/pengaturan repo dan batas verifikasi dicatat di [runbook governance](docs/runbooks/REPOSITORY_GOVERNANCE.md).

## Lisensi

Kode dan dokumentasi proyek menggunakan [MIT License](LICENSE), dipilih oleh maintainer pada 2026-11-07. Hak pihak ketiga tetap berlaku. Lisensi ini tidak otomatis melisensikan foto kiriman pengguna, merek atau logo; kebijakan foto/brand untuk penggunaan publik harus ditetapkan sebelum beta.
