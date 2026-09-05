# CHANGELOG — Instagram Reels Automation

## [2.0.0] — 2026-08-04 (Data Tables Edition)

### 🚨 Breaking Changes
- **MIGRASI TOTAL**: Semua penyimpanan data berpindah dari Airtable ke n8n Data Tables
- Tidak ada lagi ketergantungan ke Airtable API, Airtable Credential, maupun Airtable Base ID
- Struktur field diperbarui menggunakan format snake_case (contoh: `NamaAkun` → `nama_akun`)

### ✅ Ditambahkan
- **7 Data Tables** baru sebagai pengganti Airtable:
  - `InstagramAccounts` — Data akun Instagram
  - `QueueUploads` — Antrean upload
  - `UploadHistory` — Riwayat upload
  - `WorkflowSettings` — Konfigurasi global
  - `CaptionTemplates` — Template prompt AI
  - `NotificationSettings` — Pengaturan notifikasi
  - `Logs` — Log aktivitas
- Node **Log Error** — Mencatat semua error ke Data Table Logs
- Node **Log Sukses** — Mencatat semua sukses ke Data Table Logs
- Node **Cari Queue Gagal** — Mencari antrian yang masih berjalan saat error
- Node **Update Queue Gagal** — Mengupdate status antrian yang gagal
- Node **Tambah Queue** — Memasukkan video ke antrian sebelum diproses
- Node **Update Queue Publishing** — Update status antrian ke Publishing
- Node **Update Queue Selesai** — Update status antrian ke Completed
- **Tombol Telegram baru**: Lihat Reels, Lihat Cloudinary, Lihat Google Drive, Lihat Riwayat, Retry Upload
- Sticky Notes baru: Data Tables, Queue, Logging
- File `IMPORT_DATA_TABLES.md` — Dokumentasi lengkap
- 7 file CSV dengan data dummy untuk import langsung

### 🔄 Diubah
- **Aktifkan Akun** (Airtable `update`) → **Set Akun Aktif** (Data Table `updateRow`)
- **Ambil Semua Akun** (Airtable `search`) → **Ambil Semua Akun** (Data Table `getRows`)
- **Ambil Akun Untuk Ganti** (Airtable `search`) → **Ambil Akun Untuk Ganti** (Data Table `getRows`)
- **Ambil Antrian** (Airtable `search` + filterByFormula) → **Ambil Antrian** (Data Table `searchRows`)
- **Ambil Statistik** (Airtable `search`) → **Ambil Statistik** (Data Table `getRows`)
- **Ambil Riwayat** (Airtable `search` + sort) → **Ambil Riwayat** (Data Table `getRows`)
- **Ambil Akun Aktif** (Airtable `search` + filterByFormula) → **Ambil Akun Aktif** (Data Table `searchRows`)
- **Cek Antrian Berjalan** (Airtable `search` + filterByFormula) → **Cek Antrian Berjalan** (Data Table `searchRows`)
- **Simpan Data Upload** (Airtable `create`) → **Simpan Riwayat** (Data Table `insertRow`)
- **Update Status Published** (Airtable `update`) → **Update Riwayat Sukses** (Data Table `updateRow`)
- **Update Status Gagal** (Airtable `update`) → **Update Queue Gagal** (Data Table `updateRow`)
- Sticky Note Airtable → Sticky Note Data Tables
- Pesan Telegram "Kirim Instruksi Tambah Akun" — diarahkan ke n8n Data Tables (bukan Airtable)
- Pesan Telegram "Kirim Pengaturan" — diarahkan ke n8n Data Tables (bukan Airtable)
- Tombol "Lihat Airtable" → dihapus dan diganti tombol yang lebih informatif
- Semua referensi Airtable URL dihapus dari pesan Telegram
- Nama node "Format Daftar Akun" — expression diupdate dari `$json.fields.NamaAkun` ke `$json.nama_akun`
- Nama node "Set Data Akun" — semua expression field diupdate ke nama kolom baru
- Workflow dioptimasi: node-node yang tidak perlu dihapus

### 🗑️ Dihapus
- **7 node Airtable** dihapus sepenuhnya:
  1. `Aktifkan Akun` (Airtable update)
  2. `Ambil Semua Akun` (Airtable search)
  3. `Ambil Akun Untuk Ganti` (Airtable search)
  4. `Ambil Antrian` (Airtable search)
  5. `Ambil Statistik` (Airtable search)
  6. `Ambil Riwayat` (Airtable search)
  7. `Ambil Akun Aktif` (Airtable search)
  8. `Cek Antrian Berjalan` (Airtable search)
  9. `Simpan Data Upload` (Airtable create)
  10. `Update Status Published` (Airtable update)
  11. `Update Status Gagal` (Airtable update)
- Airtable Credential (`AbYqCc6ZdNGvzBSy`)
- Semua referensi Airtable Base ID (`appafQ1viq4sJXOve`)
- Semua referensi Airtable Table ID (`tblAkunMultiAccount01`, `tblBAgS2enNJfFo5R`)
- Node `Create a data table` (node percobaan yang tidak terhubung)
- Node `Get row(s)` (node percobaan yang tidak terhubung)
- URL Airtable dari pesan Telegram

### 🔧 Diperbaiki
- Error handling kini benar-benar mencari dan mengupdate antrian yang gagal di Data Tables
- Tombol Telegram error mengarah ke menu Queue dan Statistik (bukan URL Airtable)
- Flow antrian lebih konsisten — status diupdate di setiap tahap proses

---

## [1.0.0] — 2026-07-XX (Airtable Edition)

### Awal
- Workflow pertama kali dibuat menggunakan Airtable sebagai database
- Multi-akun Instagram via satu workflow
- Dashboard Telegram dengan inline keyboard
- Google Drive sebagai sumber video
- Cloudinary sebagai CDN sementara
- AI Caption menggunakan OpenAI-compatible API
- Instagram Graph API v25.0 untuk publish Reels
- Error handling dasar
