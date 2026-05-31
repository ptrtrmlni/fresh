# F.R.E.S.H Backend

REST API backend untuk platform **F.R.E.S.H (Food Resource Efficiency & Smart Handling)**, sebuah aplikasi yang membantu mengurangi food waste melalui pengelolaan inventaris makanan, marketplace, donasi makanan, serta rekomendasi berbasis AI.

Backend ini menyediakan layanan autentikasi, manajemen pengguna, inventaris, marketplace, transaksi, donasi, notifikasi, dashboard analitik, integrasi AI, dan subscription.

---

## Technology Stack

### Backend

* Node.js 18+
* Express.js 5
* JWT Authentication
* Joi Validation
* Socket.IO
* Multer

### Database

* PostgreSQL
* Supabase PostgreSQL

### Authentication & Storage

* Supabase Auth (Google OAuth)
* Supabase Storage

### Third Party Services

* Brevo Email API
* AI Recommendation Service
* AI Food Scanner Service
* AI Risk Prediction Service

### Deployment

* Railway
* Nixpacks

---

## Architecture

```text
Frontend (React + Vite)
        │
        ▼
REST API Backend (Express.js)
        │
        ▼
PostgreSQL Database (Supabase)

External Services:
- Brevo Email API
- Supabase Auth
- Supabase Storage
- AI Recommendation Service
- AI Food Scanner Service
- AI Risk Prediction Service
```

---

## Project Structure

```text
backend/
├── migrations/
├── src/
│   ├── app.js
│   ├── server.js
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── sockets/
│   ├── utils/
│   └── validator/
├── .env.example
├── package.json
├── package-lock.json
├── railway.json
├── Procfile
├── nixpacks.toml
└── README.md
```

---

## Features

### Authentication

* Register
* Login
* Google OAuth Login
* Email OTP Verification
* Forgot Password
* Reset Password

### User Management

* Profile Management
* Location Management

### Inventory Management

* Create Inventory
* Update Inventory
* Delete Inventory
* Inventory Usage Tracking
* Expiration Monitoring

### Marketplace

* Product Management
* Marketplace Listing
* Purchase Transactions
* Sales Management

### Donation System

* Food Donation
* Donation Requests
* Donation Tracking

### Dashboard & Analytics

* Personal Dashboard
* Business Dashboard
* Impact Report

### AI Features

* Food Scanner
* Risk Prediction
* Recommendation Engine

### Notification System

* Real-time Notifications
* Socket.IO Messaging

### Subscription

* Plan Management
* Checkout
* Payment Simulation

---

## Prerequisites

Sebelum menjalankan aplikasi, pastikan tersedia:

* Node.js 18 atau lebih baru
* npm
* PostgreSQL atau Supabase PostgreSQL
* Git
* Brevo Account (OTP Email)
* Supabase Project
* AI Service (opsional)

Cek versi Node.js:

```bash
node --version
npm --version
```

---

## Installation

### Clone Repository

```bash
git clone <repository-url>
cd backend
```

### Install Dependencies

```bash
npm install
```

### Setup Environment Variables

```bash
cp .env.example .env
```

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

### Configure Database

Contoh konfigurasi lokal:

```env
DATABASE_URL=postgresql://postgres:password@localhost:5432/fresh_db
```

Contoh Supabase:

```env
DATABASE_URL=postgresql://postgres.<project-ref>:<password>@aws-1-ap-northeast-1.pooler.supabase.com:6543/postgres
```

### Run Database Migration

```bash
npm run migrate:up
```

### Start Development Server

```bash
npm run dev
```

Server berjalan pada:

```text
http://localhost:3000
```

Health Check:

```text
http://localhost:3000/health
```

---

## Environment Variables

Contoh file `.env`:

```env
PORT=3000
NODE_ENV=development

DATABASE_URL=

JWT_SECRET=

CORS_ORIGIN=http://localhost:5173

EMAIL_USER=
BREVO_API_URL=https://api.brevo.com/v3/smtp/email
BREVO_API_KEY=
OTP_EXPIRED_MINUTES=5

SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
SUPABASE_STORAGE_BUCKET=uploads

AI_PREDICT_RISK_URL=
AI_SCAN_URL=
AI_RECOMMENDATION_URL=
```

---

## Available Scripts

```bash
npm run dev
```

Menjalankan server development menggunakan nodemon.

```bash
npm run start
```

Menjalankan server production.

```bash
npm run migrate
```

Menjalankan migration command.

```bash
npm run migrate:up
```

Menjalankan migrasi terbaru.

```bash
npm run migrate:down
```

Rollback migrasi terakhir.

---

## API Base URL

Development:

```text
http://localhost:3000
```

Production:

```text
https://your-backend-domain.up.railway.app
```

---

## Main API Modules

### Authentication

```text
/api/auth
```

### Users

```text
/api/users
```

### Dashboard

```text
/api/dashboard
```

### Impact

```text
/api/impact
```

### Inventories

```text
/api/inventories
```

### Inventory Outs

```text
/api/inventory-outs
```

### Donations

```text
/api/donations
```

### Donation Requests

```text
/api/donation-requests
```

### Products

```text
/api/products
```

### Marketplace

```text
/api/marketplace
```

### Transactions

```text
/api/transactions
```

### Notifications

```text
/api/notifications
```

### Scanner

```text
/api/scans
```

### Recommendations

```text
/api/recommendations
```

### Subscription

```text
/api/subscription
```

---

## Example Request

### Register

```bash
curl -X POST http://localhost:3000/api/auth/register \
-H "Content-Type: application/json" \
-d '{
  "name":"User FRESH",
  "email":"user@example.com",
  "password":"password123",
  "role":"pribadi"
}'
```

### Login

```bash
curl -X POST http://localhost:3000/api/auth/login \
-H "Content-Type: application/json" \
-d '{
  "email":"user@example.com",
  "password":"password123"
}'
```

### Get Profile

```bash
curl http://localhost:3000/api/users/profile \
-H "Authorization: Bearer <token>"
```

---

## Deployment (Railway)

Repository telah disiapkan untuk deployment menggunakan Railway.

### Build Configuration

* Builder: Nixpacks
* Start Command:

```bash
npm run migrate:up && npm run start
```

### Deployment Steps

1. Push repository ke GitHub
2. Buat project Railway
3. Hubungkan repository
4. Tambahkan environment variables
5. Deploy
6. Pastikan endpoint `/health` berjalan normal

---

## Security Best Practices

* Jangan commit file `.env`
* Jangan membagikan `JWT_SECRET`
* Jangan membagikan `BREVO_API_KEY`
* Jangan membagikan `SUPABASE_SERVICE_ROLE_KEY`
* Gunakan HTTPS pada production
* Batasi `CORS_ORIGIN` hanya ke domain frontend resmi
* Lindungi seluruh endpoint sensitif dengan JWT dan Role Guard

---

## Troubleshooting

### Database Connection Failed

Pastikan:

* DATABASE_URL benar
* Database aktif
* Credential database valid

### CORS Error

Pastikan:

```env
CORS_ORIGIN=http://localhost:5173
```

atau

```env
CORS_ORIGIN=http://localhost:5173,https://your-domain.vercel.app
```

### Unauthorized
Pastikan request mengirim:

```http
Authorization: Bearer <token>
```


## Authors

F.R.E.S.H Development Team

Capstone Project – Full Stack Web Developer
