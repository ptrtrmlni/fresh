# F.R.E.S.H Frontend

Frontend F.R.E.S.H adalah aplikasi web React untuk membantu pengguna mengurangi food waste melalui inventaris makanan, pemindai AI, rekomendasi penggunaan bahan, donasi, marketplace, transaksi, notifikasi, analytics, dan subscription. Aplikasi ini mendukung dua role utama: `pribadi` dan `bisnis`.

## Ringkasan Teknologi

- Framework UI: React 19
- Build tool: Vite
- Routing: React Router DOM
- HTTP client: Axios
- Map: Leaflet dan React Leaflet
- Icon: Lucide React
- OAuth Google: Supabase Auth
- Mock/local data opsional: JSON Server
- Linting: ESLint
- Deployment: Vercel

## Struktur Folder
```text
frontend/
|-- public/                     # Asset publik
|-- src/
|   |-- assets/                 # Gambar dan asset aplikasi
|   |-- components/             # Komponen reusable dan layout
|   |-- hooks/                  # Custom hooks
|   |-- pages/                  # Halaman aplikasi
|   |-- services/               # API client dan service per fitur
|   |-- styles/                 # File CSS per halaman/fitur
|   |-- utils/                  # Helper auth, format, geolocation, plan
|   |-- App.jsx                 # Definisi route aplikasi
|   |-- index.css               # Global style
|   `-- main.jsx                # Entry point React
|-- .env.example                # Contoh environment variable
|-- db.json                     # Data mock untuk json-server
|-- eslint.config.js            # Konfigurasi ESLint
|-- index.html
|-- package.json
|-- vercel.json                 # Konfigurasi deployment Vercel
|-- vite.config.js
`-- README.md
```

## Prasyarat

Pastikan perangkat sudah memiliki:

- Node.js 18 atau lebih baru
- npm
- Backend F.R.E.S.H yang sudah berjalan
- Akun Supabase jika ingin memakai Google Sign-In

Cek versi Node.js dan npm:

```bash
node --version
npm --version
```

## Instalasi Lokal

### 1. Masuk ke Folder Frontend

```bash
cd frontend
```

### 2. Install Dependency

```bash
npm install
```

Catatan: folder `node_modules` tidak perlu di-commit ke GitHub karena sudah masuk `.gitignore`.

### 3. Siapkan File Environment

Salin file contoh environment:

```bash
cp .env.example .env
```

Untuk Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

### 4. Isi Environment Variable

Contoh `.env` untuk development lokal:

```env
VITE_API_BASE_URL=http://localhost:3000/api
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
```

Keterangan:

| Variabel | Wajib | Fungsi |
| --- | --- | --- |
| `VITE_API_BASE_URL` | Ya | Base URL backend API. Untuk lokal biasanya `http://localhost:3000/api`. |
| `VITE_SUPABASE_URL` | Tidak | URL project Supabase untuk Google Sign-In. |
| `VITE_SUPABASE_ANON_KEY` | Tidak | Supabase anon key untuk OAuth Google di frontend. |

Jika tidak memakai Google Sign-In, Supabase env boleh dikosongkan, tetapi tombol login Google tidak akan berjalan.

### 5. Jalankan Backend

Frontend membutuhkan backend untuk auth, data inventaris, produk, transaksi, donasi, notifikasi, dan fitur lainnya.

Dari folder backend:

```bash
npm install
npm run migrate:up
npm run dev
```

Pastikan backend aktif di:

```text
http://localhost:3000
```

Health check backend:

```text
http://localhost:3000/health
```

### 6. Jalankan Frontend

Dari folder frontend:

```bash
npm run dev
```

Buka aplikasi di browser:

```text
http://localhost:5173
```

## Script NPM

| Script | Fungsi |
| --- | --- |
| `npm run dev` | Menjalankan Vite development server. |
| `npm run server` | Menjalankan JSON Server dengan `db.json` di port `5000`. Opsional untuk mock data. |
| `npm run build` | Membuat production build ke folder `dist`. |
| `npm run lint` | Menjalankan ESLint untuk mengecek kualitas kode. |
| `npm run preview` | Menjalankan preview hasil build production. |

## Cara Penggunaan Aplikasi

### 1. Landing Page

Buka:

```text
http://localhost:5173/
```

Landing page berisi pengenalan F.R.E.S.H dan akses menuju login, registrasi, serta pricing.

### 2. Registrasi

Buka halaman:

```text
/register
```

Pilih role sesuai kebutuhan:

- `pribadi`: pengguna personal yang mengelola inventaris makanan, donasi, pembelian marketplace, rekomendasi, dan analytics personal.
- `bisnis`: pengguna bisnis yang mengelola produk, penjualan, dashboard bisnis, dan analytics bisnis.

Setelah registrasi, pengguna diarahkan ke flow verifikasi OTP.

### 3. Verifikasi OTP

Buka halaman:

```text
/verify-otp
```

Masukkan OTP yang dikirim melalui email. Jika OTP tidak masuk, pastikan konfigurasi email di backend sudah benar.

### 4. Login Email dan Password

Buka halaman:

```text
/login
```

Masukkan email dan password. Setelah login berhasil, token dan data user disimpan di `localStorage`.

### 5. Login Google

Login Google memakai Supabase OAuth.

Agar fitur ini berjalan:

1. Isi `VITE_SUPABASE_URL` dan `VITE_SUPABASE_ANON_KEY` di `.env` frontend.
2. Pastikan backend memiliki konfigurasi Supabase yang sesuai.
3. Tambahkan redirect URL di Supabase Auth settings:

```text
http://localhost:5173/auth/callback
```

Untuk production, tambahkan juga URL domain Vercel:

```text
https://domain-frontend.vercel.app/auth/callback
```

### 6. Reset Password

Flow reset password tersedia melalui halaman:

```text
/forgot-password
/reset-password
```

Frontend akan meminta backend mengirim OTP reset password, lalu pengguna dapat membuat password baru.

## Route Utama

### Route Public

| Path | Halaman | Keterangan |
| --- | --- | --- |
| `/` | LandingPage | Halaman awal aplikasi. |
| `/login` | LoginPage | Login email/password dan Google. |
| `/auth/callback` | AuthCallbackPage | Callback OAuth Google dari Supabase. |
| `/register` | RegisterPage | Registrasi user. |
| `/verify-otp` | VerifyOtpPage | Verifikasi OTP. |
| `/forgot-password` | ForgotPasswordPage | Permintaan reset password. |
| `/reset-password` | ResetPasswordPage | Reset password. |
| `/pricing` | PricingPage | Daftar paket subscription. |

### Route Login Semua Role

| Path | Keterangan |
| --- | --- |
| `/checkout` | Checkout subscription, bisa diakses role `pribadi` dan `bisnis`. |
| `/checkout/success` | Halaman sukses pembayaran. |

### Route Role Pribadi

| Path | Keterangan |
| --- | --- |
| `/dashboard` | Dashboard pengguna pribadi. |
| `/inventory` | Daftar inventaris makanan. |
| `/inventory/add` | Tambah inventaris. |
| `/scanner` | Pemindai AI makanan. |
| `/rekomendasi` | Rekomendasi pemanfaatan bahan. |
| `/marketplace` | Marketplace produk makanan. |
| `/pesanan` | Daftar pesanan pengguna. |
| `/pesanan/:id` | Detail pesanan. |
| `/notifications` | Notifikasi pengguna. |
| `/donasi` | Donasi makanan. |
| `/profile` | Profil pengguna. |
| `/analytics` | Analytics personal. |

### Route Role Bisnis

| Path | Keterangan |
| --- | --- |
| `/dashboard-bisnis` | Dashboard bisnis. |
| `/produk` | Manajemen produk bisnis. |
| `/pesanan-bisnis` | Daftar pesanan/penjualan bisnis. |
| `/scanner-bisnis` | Pemindai AI untuk bisnis. |
| `/notifications-bisnis` | Notifikasi bisnis. |
| `/marketplace-bisnis` | Marketplace dari sisi bisnis. |
| `/profile-bisnis` | Profil bisnis. |
| `/analytics-bisnis` | Analytics bisnis. |

## Alur Penggunaan Role Pribadi

1. Register sebagai `pribadi` atau login dengan akun pribadi.
2. Verifikasi OTP jika akun baru.
3. Masuk ke dashboard personal.
4. Tambahkan inventaris makanan di menu Inventaris.
5. Gunakan Pemindai AI untuk scan bahan makanan melalui upload gambar.
6. Lihat rekomendasi penggunaan bahan di menu Rekomendasi.
7. Buka Marketplace untuk membeli produk dari pengguna bisnis.
8. Pantau pesanan di menu Pesanan.
9. Buat atau kelola donasi di menu Donasi.
10. Lihat dampak penggunaan aplikasi di menu Analytics.

## Alur Penggunaan Role Bisnis

1. Register sebagai `bisnis` atau login dengan akun bisnis.
2. Verifikasi OTP jika akun baru.
3. Masuk ke dashboard bisnis.
4. Tambahkan produk di menu Produk.
5. Pantau pesanan masuk di menu Pesanan Bisnis.
6. Gunakan Pemindai AI jika perlu memeriksa bahan/produk.
7. Lihat marketplace dan analytics bisnis.
8. Kelola profil bisnis di menu Profil.

## Integrasi Backend

Semua request API dipusatkan melalui:

```text
src/services/api.js
```

Axios memakai base URL dari:

```env
VITE_API_BASE_URL
```

Token login otomatis dikirim di header:

```http
Authorization: Bearer <token>
```

Jika backend mengembalikan status `401` atau `403`, frontend akan menghapus data auth dari `localStorage` dan mengarahkan user ke halaman login.

## Service Frontend

Folder `src/services` berisi service per fitur:

| File | Fungsi |
| --- | --- |
| `api.js` | Konfigurasi Axios dan interceptor auth. |
| `authService.js` | Register, login, Google login, OTP, reset password, profil. |
| `dashboardService.js` | Data dashboard personal/bisnis. |
| `inventoryService.js` | CRUD inventaris. |
| `inventoryOutService.js` | Riwayat inventaris keluar. |
| `donationService.js` | Donasi dan donation request. |
| `productService.js` | Produk bisnis. |
| `marketplaceService.js` | Produk marketplace. |
| `transactionService.js` | Pesanan dan transaksi. |
| `notificationService.js` | Notifikasi. |
| `recommendationService.js` | Rekomendasi bahan. |
| `scanService.js` | Upload gambar ke pemindai AI. |
| `impactService.js` | Data impact/analytics. |
| `subscriptionService.js` | Paket, checkout, pembayaran, subscription. |
| `supabase.js` | Konfigurasi Supabase OAuth. |

## Autentikasi dan Role Guard

Komponen route protection berada di:

```text
src/components/ProtectedRoute.jsx
```

Frontend membaca token dan user dari `localStorage`:

- `token`: JWT dari backend.
- `user`: data user login, termasuk role.

Jika user belum login, halaman protected akan redirect ke `/login`.

Jika role tidak sesuai:

- Role `bisnis` diarahkan ke `/dashboard-bisnis`.
- Role selain bisnis diarahkan ke `/dashboard`.

## Peta dan Lokasi

Frontend memakai Leaflet dan React Leaflet untuk fitur lokasi, marketplace map, donation map, dan location picker.

Komponen terkait:

- `FreshMap.jsx`
- `DonationMap.jsx`
- `MarketplaceMap.jsx`
- `LocationPicker.jsx`

Pastikan browser mengizinkan akses lokasi jika fitur lokasi digunakan.

## JSON Server Opsional

Proyek memiliki `db.json` dan script:

```bash
npm run server
```

JSON Server berjalan di:

```text
http://localhost:5000
```

Gunakan ini hanya untuk kebutuhan mock/local development. Untuk aplikasi utama, gunakan backend F.R.E.S.H melalui `VITE_API_BASE_URL`.

## Build Production

Jalankan:

```bash
npm run build
```

Hasil build berada di:

```text
dist/
```

Preview hasil build:

```bash
npm run preview
```

## Linting

Jalankan:

```bash
npm run lint
```

Gunakan lint sebelum commit untuk menangkap error umum pada React hooks, import, atau style kode.

## Deployment ke Vercel

Frontend sudah memiliki `vercel.json` dengan konfigurasi:

- Framework: Vite
- Build command: `npm run build`
- Output directory: `dist`
- SPA rewrite ke `index.html`
- Cache header untuk asset

Langkah deployment:

1. Push folder frontend ke GitHub.
2. Import project di Vercel.
3. Jika repository berisi backend dan frontend, set root directory ke:

```text
frontend
```

4. Set environment variable production:

```env
VITE_API_BASE_URL=https://url-backend-production/api
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
```

5. Deploy.
6. Pastikan backend mengizinkan domain Vercel melalui `CORS_ORIGIN`.

Contoh backend CORS production:

```env
CORS_ORIGIN=https://domain-frontend.vercel.app
```

Jika memakai preview deployment Vercel, masukkan beberapa origin dengan koma di backend:

```env
CORS_ORIGIN=http://localhost:5173,https://domain-frontend.vercel.app,https://preview-url.vercel.app
```

## Troubleshooting

### Halaman blank setelah deploy

Pastikan:

- Build berhasil.
- `vercel.json` ikut terdeploy.
- Output directory Vercel adalah `dist`.
- Tidak ada error JavaScript di browser console.

### API tidak terpanggil

Periksa:

- `VITE_API_BASE_URL` sudah benar.
- Backend berjalan dan endpoint `/health` aktif.
- Backend mengizinkan origin frontend melalui `CORS_ORIGIN`.
- Environment variable sudah di-set sebelum build.

### Setelah mengubah `.env`, nilai belum berubah

Restart dev server:

```bash
npm run dev
```

Vite membaca environment variable saat server dimulai.

### Login selalu kembali ke halaman login

Periksa:

- Endpoint `/api/auth/login` mengembalikan token dan user.
- Token tersimpan di `localStorage` dengan key `token`.
- User tersimpan di `localStorage` dengan key `user`.
- Role user adalah `pribadi` atau `bisnis`.

### Error 401 atau 403

Kemungkinan token tidak valid, token kedaluwarsa, atau role tidak sesuai. Login ulang dan pastikan akun memiliki role yang benar.

### Login Google gagal

Periksa:

- `VITE_SUPABASE_URL` dan `VITE_SUPABASE_ANON_KEY` sudah benar.
- Redirect URL `/auth/callback` sudah didaftarkan di Supabase.
- Backend memiliki konfigurasi Supabase untuk memverifikasi token.
- Dev server sudah direstart setelah mengubah `.env`.

### Map tidak muncul sempurna

Periksa:

- Dependency `leaflet` dan `react-leaflet` sudah terinstall.
- CSS Leaflet diimport sesuai kebutuhan komponen.
- Browser tidak memblokir permission lokasi.
