# F.R.E.S.H - Food Resource Efficiency & Smart Handling

F.R.E.S.H adalah aplikasi web untuk membantu mengurangi food waste melalui pengelolaan inventaris makanan, prediksi risiko kedaluwarsa, pemindai AI, rekomendasi pemanfaatan bahan, donasi makanan, marketplace, transaksi, notifikasi, analytics, dan subscription.

Aplikasi ini dirancang untuk dua jenis pengguna:

- `pribadi`: pengguna personal yang ingin mengelola bahan makanan agar tidak terbuang.
- `bisnis`: pelaku usaha yang ingin menjual, mengelola, atau mendistribusikan produk makanan dengan lebih efisien.

## Gambaran Aplikasi

F.R.E.S.H menghubungkan pengguna pribadi dan bisnis dalam satu ekosistem pangan. Pengguna pribadi dapat mencatat bahan makanan, memantau bahan yang mendekati kedaluwarsa, menerima rekomendasi penggunaan, membeli produk di marketplace, serta membuat donasi. Pengguna bisnis dapat mengelola produk, menerima pesanan, memantau performa bisnis, dan menggunakan fitur analytics.

Aplikasi ini terdiri dari tiga bagian utama:

| Bagian | Teknologi | Fungsi |
| --- | --- | --- |
| Frontend | React, Vite, React Router, Axios, Leaflet | Antarmuka web untuk pengguna. |
| Backend | Node.js, Express, PostgreSQL, JWT, Socket.IO | REST API, autentikasi, data aplikasi, transaksi, notifikasi, dan realtime message. |
| AI Service | FastAPI, TensorFlow, Gemini API | Pemindai makanan, prediksi risiko, dan rekomendasi berbasis AI/fallback heuristic. |

## Fitur Utama

### Untuk Pengguna Pribadi

- Registrasi, login, OTP, reset password, dan login Google.
- Dashboard personal.
- Manajemen inventaris makanan.
- Prediksi risiko makanan berdasarkan data inventaris.
- Pemindai AI melalui upload gambar makanan.
- Rekomendasi pemanfaatan bahan makanan.
- Marketplace produk makanan.
- Pesanan dan detail transaksi.
- Donasi makanan dan permintaan donasi.
- Notifikasi.
- Analytics dampak penggunaan aplikasi.
- Pengelolaan profil dan lokasi.

### Untuk Pengguna Bisnis

- Registrasi dan login sebagai akun bisnis.
- Dashboard bisnis.
- Manajemen produk marketplace.
- Pengelolaan pesanan/penjualan.
- Pemindai AI.
- Marketplace dari sisi bisnis.
- Notifikasi bisnis.
- Analytics bisnis.
- Pengelolaan profil bisnis.

### Fitur AI

- Scan gambar makanan.
- Prediksi jenis makanan.
- Prediksi risiko dan kondisi makanan.
- Rekomendasi pemanfaatan bahan.
- Fallback heuristic jika service AI eksternal tidak tersedia.

## Struktur Project

```text
fresh-app-main/
|-- AI/                         # FastAPI AI service
|   |-- data/                   # Dataset dan data pendukung
|   |-- model/                  # Model AI dan metadata
|   |-- services/               # Logic prediksi, scan, nutrisi, rekomendasi
|   |-- main.py                 # Entry point FastAPI
|   |-- requirements.txt
|   `-- .env.example
|-- backend/                    # Express REST API
|   |-- migrations/             # Migrasi database PostgreSQL
|   |-- src/                    # App, route, controller, service, model, middleware
|   |-- package.json
|   |-- railway.json
|   `-- README.md
|-- frontend/                   # React web app
|   |-- public/
|   |-- src/                    # Components, pages, services, styles, utils
|   |-- package.json
|   |-- vercel.json
|   `-- README.md
|-- .gitignore
`-- README.md
```

Dokumentasi lebih detail tersedia di:

- [Frontend README](frontend/README.md)
- [Backend README](backend/README.md)

## Prasyarat

Pastikan perangkat sudah memiliki:

- Node.js 18 atau lebih baru.
- npm.
- Python 3.11 atau versi yang sesuai dengan konfigurasi AI service.
- PostgreSQL atau Supabase Postgres.
- Git.
- Akun Supabase jika ingin memakai Google Sign-In dan storage.
- API key Brevo jika ingin mengirim OTP email.
- API key Gemini jika ingin memakai rekomendasi AI berbasis Gemini.

## Cara Instalasi dan Menjalankan Project

### 1. Clone atau Ekstrak Project

Jika dari GitHub:

```bash
git clone <url-repository>
cd fresh-app-main
```

Jika dari ZIP, ekstrak file ZIP lalu masuk ke folder project.

### 2. Jalankan Backend

Masuk ke folder backend:

```bash
cd backend
npm install
Copy-Item .env.example .env
```

Untuk macOS/Linux, gunakan:

```bash
cp .env.example .env
```

Edit file `.env`, minimal isi:

```env
PORT=3000
NODE_ENV=development
DATABASE_URL=postgresql://postgres:<password>@localhost:5432/fresh_db
CORS_ORIGIN=http://localhost:5173
JWT_SECRET=replace-with-a-long-random-string
```

Jalankan migrasi database:

```bash
npm run migrate:up
```

Jalankan backend:

```bash
npm run dev
```

Backend berjalan di:

```text
http://localhost:3000
```

Health check:

```text
http://localhost:3000/health
```

### 3. Jalankan AI Service

Buka terminal baru, masuk ke folder AI:

```bash
cd AI
Copy-Item .env.example .env
```

Untuk macOS/Linux:

```bash
cp .env.example .env
```

Isi `.env` jika ingin memakai Gemini:

```env
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-2.5-flash
```

Buat virtual environment dan install dependency:

```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Untuk macOS/Linux:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Jalankan AI service:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

AI service berjalan di:

```text
http://localhost:8000
```

Endpoint penting:

```text
GET  /health
POST /scan
POST /predict-risk
POST /recommend
```

### 4. Hubungkan Backend ke AI Service

Pastikan file `backend/.env` memiliki URL AI berikut:

```env
AI_PREDICT_RISK_URL=http://localhost:8000/predict-risk
AI_SCAN_URL=http://localhost:8000/scan
AI_RECOMMENDATION_URL=http://localhost:8000/recommend
```

Restart backend setelah mengubah `.env`.

### 5. Jalankan Frontend

Buka terminal baru, masuk ke folder frontend:

```bash
cd frontend
npm install
Copy-Item .env.example .env
```

Untuk macOS/Linux:

```bash
cp .env.example .env
```

Isi `.env` frontend:

```env
VITE_API_BASE_URL=http://localhost:3000/api
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
```

Jika belum memakai Google Sign-In, Supabase env boleh dikosongkan.

Jalankan frontend:

```bash
npm run dev
```

Frontend berjalan di:

```text
http://localhost:5173
```

## Urutan Menjalankan Aplikasi Lokal

Gunakan tiga terminal terpisah:

Terminal 1, backend:

```bash
cd backend
npm run dev
```

Terminal 2, AI service:

```bash
cd AI
.\.venv\Scripts\Activate.ps1
uvicorn main:app --host 0.0.0.0 --port 8000
```

Terminal 3, frontend:

```bash
cd frontend
npm run dev
```

Lalu buka:

```text
http://localhost:5173
```

## Cara Penggunaan Aplikasi

### 1. Membuka Aplikasi

Buka browser dan akses:

```text
http://localhost:5173
```

Pengguna akan melihat landing page F.R.E.S.H.

### 2. Membuat Akun

1. Klik menu registrasi.
2. Isi data akun.
3. Pilih role `pribadi` atau `bisnis`.
4. Kirim registrasi.
5. Masukkan OTP pada halaman verifikasi.
6. Setelah verifikasi berhasil, login ke aplikasi.

### 3. Login

Pengguna dapat login menggunakan:

- Email dan password.
- Google Sign-In jika Supabase OAuth sudah dikonfigurasi.

Setelah login, aplikasi akan mengarahkan pengguna sesuai role:

- Role `pribadi` diarahkan ke dashboard personal.
- Role `bisnis` diarahkan ke dashboard bisnis.

## Penggunaan Role Pribadi

### Dashboard

Dashboard personal menampilkan ringkasan aktivitas, kondisi inventaris, rekomendasi, dan informasi penting lainnya.

### Inventaris Makanan

Pengguna dapat:

1. Menambahkan bahan makanan.
2. Mengisi nama bahan, kategori, jumlah, lokasi penyimpanan, dan tanggal terkait.
3. Melihat daftar inventaris.
4. Mengubah data inventaris.
5. Menghapus inventaris.
6. Mencatat bahan yang sudah digunakan atau keluar.

### Pemindai AI

1. Buka menu pemindai.
2. Upload gambar makanan.
3. Aplikasi mengirim gambar ke backend dan AI service.
4. Hasil scan menampilkan prediksi makanan, risiko, atau rekomendasi sesuai data yang tersedia.

### Rekomendasi

Menu rekomendasi membantu pengguna menentukan cara memanfaatkan bahan makanan agar tidak terbuang.

### Marketplace

Pengguna pribadi dapat:

1. Melihat produk dari akun bisnis.
2. Membuka detail produk.
3. Membuat transaksi pembelian.
4. Melihat riwayat pesanan.

### Donasi

Pengguna dapat membuat donasi makanan atau mengelola permintaan donasi. Fitur ini membantu makanan yang masih layak dikonsumsi tersalurkan ke pihak lain.

### Analytics

Analytics menampilkan dampak penggunaan aplikasi, misalnya aktivitas pengurangan food waste atau insight terkait penggunaan bahan.

### Profil dan Lokasi

Pengguna dapat mengubah profil serta menyimpan lokasi. Lokasi dipakai untuk fitur berbasis peta seperti marketplace dan donasi.

## Penggunaan Role Bisnis

### Dashboard Bisnis

Dashboard bisnis menampilkan ringkasan performa bisnis, produk, transaksi, dan aktivitas terkait.

### Manajemen Produk

Pengguna bisnis dapat:

1. Menambahkan produk.
2. Mengisi nama, harga, stok, status, dan informasi produk.
3. Mengubah produk.
4. Mengubah status produk.
5. Menghapus produk.

### Pesanan Bisnis

Menu pesanan bisnis digunakan untuk memantau pembelian dari pengguna pribadi dan memperbarui status transaksi.

### Marketplace Bisnis

Pengguna bisnis dapat melihat tampilan marketplace dan memastikan produk tampil dengan benar.

### Analytics Bisnis

Analytics bisnis membantu melihat performa dan dampak bisnis di platform F.R.E.S.H.

### Profil Bisnis

Pengguna bisnis dapat mengelola informasi akun dan lokasi bisnis.

## Subscription dan Pricing

Aplikasi memiliki halaman pricing dan subscription. Pengguna dapat:

1. Melihat daftar paket.
2. Masuk ke checkout.
3. Melakukan simulasi pembayaran.
4. Melihat status subscription.
5. Membatalkan subscription jika tersedia di flow aplikasi.

## Environment Penting

### Frontend

```env
VITE_API_BASE_URL=http://localhost:3000/api
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
```

### Backend

```env
PORT=3000
DATABASE_URL=postgresql://postgres:<password>@localhost:5432/fresh_db
CORS_ORIGIN=http://localhost:5173
JWT_SECRET=replace-with-a-long-random-string
AI_PREDICT_RISK_URL=http://localhost:8000/predict-risk
AI_SCAN_URL=http://localhost:8000/scan
AI_RECOMMENDATION_URL=http://localhost:8000/recommend
```

### AI Service

```env
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-2.5-flash
```

## Deployment Singkat

### Frontend

Frontend dapat dideploy ke Vercel.

- Root directory: `frontend`
- Build command: `npm run build`
- Output directory: `dist`
- Environment production: `VITE_API_BASE_URL`, `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`

### Backend

Backend dapat dideploy ke Railway, Render, Fly.io, atau VPS.

- Root directory: `backend`
- Start command: `npm run migrate:up && npm run start`
- Health check: `/health`
- Environment production: database, CORS, JWT, email, Supabase, dan AI URL

### AI Service

AI service dapat dideploy ke Railway atau platform Python/FastAPI lain.

- Root directory: `AI`
- Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
- Health check: `/health`
- Environment production: `GEMINI_API_KEY`, `GEMINI_MODEL`

## Troubleshooting

### Frontend tidak bisa mengambil data

Periksa:

- Backend berjalan di `http://localhost:3000`.
- `VITE_API_BASE_URL` sudah benar.
- Backend mengizinkan origin frontend lewat `CORS_ORIGIN`.
- Dev server frontend sudah direstart setelah mengubah `.env`.

### Login gagal

Periksa:

- Backend aktif.
- Database sudah dimigrasi.
- Email dan password benar.
- JWT secret tersedia di backend.
- Jika memakai Google, konfigurasi Supabase frontend dan backend sudah benar.

### OTP tidak masuk

Periksa:

- `BREVO_API_KEY` sudah benar.
- Email sender sudah valid.
- Backend dapat mengakses Brevo API.
- Cek log backend untuk pesan error OTP.

### Fitur AI tidak berjalan

Periksa:

- AI service berjalan di `http://localhost:8000`.
- Endpoint `/health` AI service aktif.
- URL AI di `backend/.env` sudah benar.
- Model dan dependency Python sudah tersedia.
- Backend sudah direstart setelah mengubah URL AI.

### Error CORS

Sesuaikan `CORS_ORIGIN` di backend.

Contoh lokal:

```env
CORS_ORIGIN=http://localhost:5173
```

Contoh production dengan beberapa origin:

```env
CORS_ORIGIN=http://localhost:5173,https://domain-frontend.vercel.app
```

### Migrasi database gagal

Periksa:

- `DATABASE_URL` benar.
- Database aktif.
- User database punya permission membuat tabel.
- Jalankan ulang `npm run migrate:up` dari folder backend.