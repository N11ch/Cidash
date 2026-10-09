# CiDash — Backend Service 🚀
> REST API, Authentication, and Business Logic Engine for **CiDash (Circle Dashboard)**.

Dibangun dengan **NestJS 12**, **Prisma ORM 5**, dan **PostgreSQL (Supabase)**.

---

## 🛠️ Tech Stack & Modul

* **Framework:** [NestJS](https://nestjs.com/) (Node.js + TypeScript)
* **ORM:** [Prisma ORM](https://prisma.io/)
* **Database:** PostgreSQL (Supabase)
* **Autentikasi:** Passport JWT (`@nestjs/jwt`, `passport-jwt`, `bcrypt`)
* **Validasi Data:** `class-validator` & `class-transformer`

---

## 🗄️ Model Basis Data (Prisma Schema)

Model Prisma di [`prisma/schema.prisma`](file:///D:/CiDash/backend/prisma/schema.prisma) dirancang selaras dengan spesifikasi proyek:

| Model Prisma | Tabel Spesifikasi | Deskripsi |
| :--- | :--- | :--- |
| `User` | `users` | Akun pengguna, password hash, rank point, dan tiket perisai (*shield tickets*). |
| `Squad` | `groups` | Circle privat pengguna dengan `inviteCode` unik (8 karakter). |
| `SquadMember` | `group_members` | Relasi keanggotaan *many-to-many* antara User dan Squad beserta perannya (`MEMBER`, `CAPTAIN`). |
| `Quest` | `missions` | Misi olahraga dan *Bomb Mission* P2P dengan taruhan poin (*stakesPoints*). |
| `Activity` | `activity_logs` | Log perekaman GPS: jarak meter, durasi detik, pace rata-rata, dan `routeGeoJson`. |
| `PointLedger` | `coin_transactions` | Catatan transaksi poin / koin (`QUEST_WIN`, `QUEST_PENALTY`, `PURCHASE_SHIELD`). |

---

## 🔌 Endpoint API Utama

### 1. Autentikasi (`/auth`)
* `POST /auth/register` — Mendaftarkan pengguna baru (`email`, `password`, `name`).
* `POST /auth/login` — Autentikasi dan penerbitan Bearer Token JWT.
* `GET /auth/me` — Membaca profil pengguna terotentikasi saat ini (*Protected by JWT Guard*).

### 2. Lingkaran Pertemanan / Circle (`/squads`) *(Dalam Pengembangan)*
* `POST /squads` — Membuat circle baru dan meng-generate kode 8 karakter.
* `POST /squads/join` — Bergabung ke circle menggunakan kode undangan.
* `GET /squads/my` — Mengambil daftar grup yang diikuti.

### 3. Quest & Bomb Mission (`/quests`) *(Dalam Pengembangan)*
* `POST /quests` — Mengirimkan *Bomb Mission* ke teman satu grup.
* `GET /quests/active` — Daftar misi aktif yang belum kedaluwarsa.

### 4. Aktivitas GPS (`/activities`) *(Dalam Pengembangan)*
* `POST /activities` — Mengirimkan hasil perekaman GPS untuk verifikasi anti-cheat dan penyelesaian misi.

---

## 💻 Panduan Menjalankan Backend

### Prasyarat:
* Node.js v20+ & npm
* PostgreSQL connection URL

### 1. Instalasi Dependensi
```bash
npm install
```

### 2. Pengaturan `.env`
Pastikan file `.env` sudah memuat variabel berikut:
```env
DATABASE_URL="postgresql://username:password@localhost:5432/cidash?schema=public"
JWT_SECRET="your-jwt-secret-key"
PORT=3000
```

### 3. Migrasi & Generate Prisma Client
```bash
npx prisma generate
npx prisma db push
```

### 4. Menjalankan Server
```bash
# Mode Development (Hot-Reload)
npm run start:dev

# Mode Production
npm run build
npm run start:prod
```

### 5. Testing & Code Quality
```bash
# Menjalankan Linter
npm run lint

# Menjalankan Unit Test
npm run test

# Menjalankan E2E Test
npm run test:e2e
```

---

## 📖 Dokumentasi Lengkap
Spesifikasi lengkap, skenario pitch demo, dan Business Model Canvas dapat diakses di:  
👉 [Dokumentasi Spesifikasi CiDash](../docs/SPESIFIKASI_PROYEK_CIDASH.md)
