# 📋 IMPORT DATA TABLES — Instagram Reels Automation 2026

## Daftar Tabel

| No | Nama Tabel | Fungsi |
|----|-----------|--------|
| 1 | InstagramAccounts | Menyimpan seluruh akun Instagram yang dikelola |
| 2 | QueueUploads | Antrean upload video yang sedang diproses |
| 3 | UploadHistory | Riwayat seluruh upload yang telah dilakukan |
| 4 | WorkflowSettings | Konfigurasi global workflow |
| 5 | CaptionTemplates | Template prompt untuk AI caption generator |
| 6 | NotificationSettings | Pengaturan notifikasi Telegram per pengguna |
| 7 | Logs | Catatan aktivitas dan error seluruh workflow |

---

## Fungsi Tabel

### InstagramAccounts
Tabel utama yang menyimpan konfigurasi setiap akun Instagram. Satu baris = satu akun.
Field `aktif = true` menentukan akun mana yang sedang digunakan oleh workflow otomatis.

### QueueUploads
Tabel antrian upload. Setiap video yang dipilih akan dimasukkan ke tabel ini terlebih dahulu
dengan status `Downloading`, kemudian berubah menjadi `Generating Caption`, `Publishing`, `Completed`, atau `Failed`.

### UploadHistory
Tabel riwayat permanen. Setiap upload yang berhasil akan disimpan di sini beserta metadata lengkap
termasuk Post ID Instagram, URL Reels, dan URL Cloudinary.

### WorkflowSettings
Tabel konfigurasi global. Ubah nilai di sini untuk mengubah perilaku workflow tanpa harus
membuka editor n8n.

### CaptionTemplates
Kumpulan template prompt AI. Dapat dipilih per akun melalui field `prompt_ai` di InstagramAccounts.

### NotificationSettings
Konfigurasi penerima notifikasi Telegram. Mendukung beberapa Chat ID berbeda.

### Logs
Log aktivitas otomatis. Setiap sukses dan error dicatat secara permanen untuk audit dan debugging.

---

## Struktur Kolom

### InstagramAccounts
| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id | Number (auto) | ID unik |
| nama_akun | String | Nama tampilan akun |
| username | String | Username Instagram (tanpa @) |
| instagram_business_id | String | Instagram Business Account ID dari Meta |
| facebook_page_id | String | Facebook Page ID yang terhubung |
| access_token | String | Meta Graph API Access Token |
| folder_google_drive | String | ID folder Google Drive sumber video |
| folder_arsip | String | ID folder Google Drive untuk arsip setelah upload |
| endpoint_ai | String | URL endpoint AI (OpenAI-compatible) |
| model_ai | String | Nama model AI (contoh: gpt-4o-mini) |
| prompt_ai | String | Template prompt untuk generate caption |
| aktif | String | "true" atau "false" |
| status | String | Status akun (Aktif / Nonaktif) |
| created_at | String | Timestamp ISO 8601 |
| updated_at | String | Timestamp ISO 8601 |

### QueueUploads
| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id | Number (auto) | ID unik |
| instagram_account | String | Nama akun (dari InstagramAccounts.nama_akun) |
| file_name | String | Nama file video |
| file_id | String | ID file di Google Drive |
| status | String | Waiting / Downloading / Generating Caption / Publishing / Completed / Failed |
| retry | String | Jumlah percobaan ulang |
| created_at | String | Timestamp ISO 8601 |
| updated_at | String | Timestamp ISO 8601 |

### UploadHistory
| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id | Number (auto) | ID unik |
| instagram_account | String | Nama akun |
| file_name | String | Nama file video |
| caption | String | Caption yang di-generate AI |
| instagram_post_id | String | Media ID dari Instagram setelah publish |
| reel_url | String | URL publik Reels di Instagram |
| cloudinary_url | String | URL CDN Cloudinary (sementara) |
| status | String | Publishing / Completed / Failed |
| duration | String | Durasi upload dalam detik |
| created_at | String | Timestamp ISO 8601 |

### WorkflowSettings
| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id | Number (auto) | ID unik |
| nama_setting | String | Nama parameter |
| nilai | String | Nilai parameter |
| keterangan | String | Penjelasan parameter |

### CaptionTemplates
| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id | Number (auto) | ID unik |
| nama_template | String | Nama template |
| prompt | String | Isi prompt untuk AI |
| aktif | String | "true" atau "false" |

### NotificationSettings
| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id | Number (auto) | ID unik |
| telegram_chat_id | String | Chat ID Telegram |
| telegram_bot | String | Nama kredensial bot Telegram di n8n |
| notify_success | String | Kirim notif saat sukses ("true"/"false") |
| notify_failed | String | Kirim notif saat gagal ("true"/"false") |
| notify_progress | String | Kirim notif progres ("true"/"false") |

### Logs
| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id | Number (auto) | ID unik |
| workflow | String | Nama workflow |
| level | String | INFO / WARNING / ERROR |
| pesan | String | Pesan log |
| waktu | String | Timestamp ISO 8601 |

---

## Cara Import

### Langkah 1 — Buka n8n Data Tables
1. Login ke n8n Self-Hosted
2. Klik menu **Settings** (ikon roda gigi)
3. Pilih **Data** → **Tables**

### Langkah 2 — Buat Tabel Baru
1. Klik tombol **+ New Table**
2. Masukkan nama tabel (contoh: `InstagramAccounts`)
3. Klik **Create**

### Langkah 3 — Import CSV
1. Setelah tabel terbuat, klik tombol **Import**
2. Upload file CSV yang sesuai (contoh: `InstagramAccounts.csv`)
3. Pastikan delimiter menggunakan **koma (,)**
4. Klik **Confirm Import**

### Langkah 4 — Verifikasi
1. Pastikan semua kolom terbaca dengan benar
2. Pastikan data dummy telah masuk ke tabel
3. Ulangi untuk semua 7 tabel

---

## Urutan Import (WAJIB DIIKUTI)

1. ✅ `WorkflowSettings.csv` — Konfigurasi dasar
2. ✅ `CaptionTemplates.csv` — Template AI
3. ✅ `NotificationSettings.csv` — Pengaturan notifikasi
4. ✅ `InstagramAccounts.csv` — Data akun (referensi utama)
5. ✅ `QueueUploads.csv` — Antrean upload
6. ✅ `UploadHistory.csv` — Riwayat upload
7. ✅ `Logs.csv` — Log aktivitas

---

## Cara Menambah Akun Baru

1. Buka n8n → Settings → Data → Tables → **InstagramAccounts**
2. Klik **+ Insert Row**
3. Isi semua kolom berikut:
   - `nama_akun`: Nama akun yang mudah dikenali
   - `username`: Username Instagram (tanpa @)
   - `instagram_business_id`: Dapatkan dari Meta Business Suite
   - `facebook_page_id`: ID halaman Facebook yang terhubung
   - `access_token`: Token dari Meta Graph API Explorer
   - `folder_google_drive`: ID folder Google Drive (sumber video)
   - `folder_arsip`: ID folder Google Drive (arsip setelah upload)
   - `endpoint_ai`: URL API AI (contoh: `https://api.openai.com/v1/chat/completions`)
   - `model_ai`: Nama model (contoh: `gpt-4o-mini`)
   - `prompt_ai`: Template prompt caption
   - `aktif`: `false` (aktifkan melalui menu Telegram)
   - `status`: `Nonaktif`
   - `created_at`: `{{ $now.toISO() }}`
   - `updated_at`: `{{ $now.toISO() }}`

---

## Cara Mengganti Akun Aktif

### Via Telegram (Direkomendasikan)
1. Kirim `/menu` ke bot Telegram
2. Tekan tombol **🔀 Ganti Akun Aktif**
3. Pilih nama akun yang ingin diaktifkan
4. Workflow akan otomatis mengupdate field `aktif = true`

### Via Data Tables (Manual)
1. Buka InstagramAccounts
2. Set semua baris field `aktif` menjadi `false`
3. Set baris akun yang diinginkan field `aktif` menjadi `true`
4. Update field `updated_at` dengan timestamp terkini

---

## Cara Mengganti Endpoint AI

1. Buka InstagramAccounts
2. Cari baris akun yang ingin diubah
3. Edit field `endpoint_ai` dengan URL endpoint baru
4. Contoh endpoint yang kompatibel:
   - OpenAI: `https://api.openai.com/v1/chat/completions`
   - Groq: `https://api.groq.com/openai/v1/chat/completions`
   - Ollama (lokal): `http://localhost:11434/v1/chat/completions`
   - OpenRouter: `https://openrouter.ai/api/v1/chat/completions`

---

## Cara Mengganti Prompt AI

1. Buka InstagramAccounts
2. Cari baris akun yang ingin diubah
3. Edit field `prompt_ai` dengan prompt baru

**Atau gunakan CaptionTemplates:**
1. Buka tabel CaptionTemplates
2. Tambah atau edit template yang diinginkan
3. Copy teks prompt ke field `prompt_ai` di InstagramAccounts

**Contoh prompt yang efektif:**
```
Buat satu caption singkat gaya hidup sehari-hari, maksimal 25 kata,
tanpa menjelaskan isi video, tambahkan 3 hashtag relevan.
```

---

## Cara Backup Data Tables

### Export Manual
1. Buka n8n → Settings → Data → Tables
2. Pilih tabel yang ingin di-backup
3. Klik tombol **Export** → **CSV**
4. Simpan file CSV

### Backup Semua Tabel (Urutan)
1. `InstagramAccounts.csv`
2. `QueueUploads.csv`
3. `UploadHistory.csv`
4. `WorkflowSettings.csv`
5. `CaptionTemplates.csv`
6. `NotificationSettings.csv`
7. `Logs.csv`

### Jadwal Backup Disarankan
- **Harian**: Logs
- **Mingguan**: QueueUploads, UploadHistory
- **Bulanan**: Semua tabel
- **Setiap perubahan konfigurasi**: WorkflowSettings, InstagramAccounts

---

## Catatan Penting

> **PENTING**: Pastikan nama tabel di n8n SAMA PERSIS dengan nama yang tertera di dokumentasi ini.
> Workflow menggunakan referensi nama tabel (bukan ID), sehingga perbedaan huruf besar/kecil akan menyebabkan error.

> **PERINGATAN**: Jangan hapus baris yang memiliki status `Downloading`, `Publishing`, atau `Generating Caption` karena proses upload sedang berjalan.

> **INFO**: Field `id` diisi otomatis oleh n8n Data Tables. Tidak perlu mengisi secara manual.
