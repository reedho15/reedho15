# PRD — Website Portfolio Ridho Alba

| Item | Detail |
|---|---|
| **Nama Produk** | Portfolio Website — Ridho Alba |
| **Versi Dokumen** | 1.0 |
| **Status** | Draft |
| **Pemilik** | Ridho Alba |
| **GitHub** | https://github.com/reedho15 |
| **LinkedIn** | https://www.linkedin.com/in/ridho-alba-bb2a55431/ |

---

## 1. Ringkasan

Website portfolio pribadi yang sederhana namun modern, berfungsi sebagai etalase profil, proyek, dan tulisan (blog) Ridho Alba. Aplikasi dibangun dengan arsitektur terpisah: **frontend** (Next.js + TypeScript + Tailwind CSS) dan **backend** (Laravel REST API). Dilengkapi **chat AI mengambang** di seluruh halaman, **dark mode**, dan **panel admin** untuk mengelola konten.

## 2. Latar Belakang & Tujuan

### Latar Belakang
Ridho adalah lulusan D3 Teknik Komputer yang memiliki minat pada Linux, jaringan, pengembangan web/mobile, dan IoT. Dibutuhkan satu tempat terpusat untuk menampilkan kemampuan, proyek, dan tulisan teknis kepada calon pemberi kerja, klien, maupun komunitas.

### Tujuan
1. Menampilkan identitas profesional dan keahlian secara menarik.
2. Menyediakan tempat publikasi proyek dan artikel blog.
3. Memudahkan pengunjung menghubungi Ridho.
4. Memberi pengalaman interaktif lewat chat AI yang menjawab pertanyaan seputar profil dan proyek.
5. Konten dapat dikelola sendiri tanpa mengubah kode (panel admin).

### Metrik Keberhasilan
- Skor Lighthouse ≥ 90 (Performance, Accessibility, SEO, Best Practices).
- Waktu muat halaman utama (LCP) < 2,5 detik.
- Seluruh halaman responsif (mobile, tablet, desktop).
- Admin dapat menambah/mengubah/menghapus proyek dan blog tanpa menyentuh kode.

## 3. Profil Pemilik (Konten Awal)

**Pendidikan**
- **D3 Teknik Komputer** — Politeknik Negeri Jember
- **TKJ (Teknik Komputer dan Jaringan)** — SMKS Nuris Jember, 2018–2021

**Minat & Keahlian**
- Linux
- Jaringan komputer
- Pengembangan web & aplikasi mobile
- IoT (Internet of Things)
- Dan teknologi lain yang berkembang

**Tautan Sosial**
- GitHub: `github.com/reedho15`
- LinkedIn: `linkedin.com/in/ridho-alba-bb2a55431`

## 4. Target Pengguna

| Persona | Kebutuhan |
|---|---|
| **Rekruter / HRD** | Melihat profil, pendidikan, skill, dan proyek dengan cepat |
| **Klien / Kolaborator** | Menilai kualitas kerja dan menghubungi Ridho |
| **Sesama developer / pembaca blog** | Membaca artikel teknis (Linux, jaringan, web, IoT) |
| **Admin (Ridho)** | Mengelola konten dan pesan masuk dengan mudah |

## 5. Ruang Lingkup

### Termasuk (In Scope)
- Halaman publik: Beranda, Blog (daftar + detail), Project (daftar + detail), Kontak.
- Chat AI mengambang di semua halaman publik.
- Dark mode / light mode.
- Panel admin (login, CRUD blog, CRUD project, kelola profil/skill, lihat pesan kontak).
- REST API Laravel.
- SEO dasar (meta tag, sitemap, Open Graph).

### Tidak Termasuk (Out of Scope — versi 1.0)
- Sistem komentar blog.
- Multi-user / multi-role admin.
- Multi-bahasa (i18n).
- Pembayaran atau e-commerce.
- Aplikasi mobile native.

## 6. Tech Stack

| Layer | Teknologi |
|---|---|
| **Frontend** | Next.js (App Router), React, TypeScript, Tailwind CSS |
| **Backend** | Laravel (REST API), PHP 8.2+ |
| **Database** | MySQL / MariaDB (alternatif: PostgreSQL) |
| **Autentikasi** | Laravel Sanctum (token-based untuk admin) |
| **AI Chat** | Anthropic Claude API (dipanggil dari backend Laravel; API key tidak diekspos ke browser) |
| **Penyimpanan File** | Laravel Storage (local / S3-compatible) |
| **UI Helper** | `next-themes` (dark mode), `lucide-react` (ikon), `framer-motion` (animasi ringan) |
| **Markdown** | `react-markdown` / editor Markdown untuk konten blog |
| **Deployment** | Frontend: Vercel / VPS; Backend: VPS (Nginx + PHP-FPM) |

## 7. Arsitektur Sistem

```
┌──────────────────────┐        HTTPS / JSON        ┌───────────────────────┐
│  Frontend (Next.js)  │ ─────────────────────────▶ │   Backend (Laravel)   │
│  TypeScript+Tailwind │ ◀───────────────────────── │      REST API         │
└──────────────────────┘                            └──────────┬────────────┘
        │                                                      │
        │  Chat widget                                         ├──▶ Database (MySQL)
        └──────────────▶ POST /api/chat ───────────────────────├──▶ Storage (gambar)
                                                               └──▶ Claude API (AI)
```

Prinsip:
- Frontend hanya berkomunikasi dengan Laravel API.
- API key AI disimpan di `.env` backend.
- Konten publik memakai SSR/ISR di Next.js agar cepat dan ramah SEO.
- CORS dikonfigurasi hanya untuk domain frontend.

## 8. Kebutuhan Fungsional

### 8.1 Halaman Publik

#### A. Beranda (`/`)
- **Hero**: nama, headline singkat, tombol CTA ("Lihat Project", "Hubungi Saya"), tautan GitHub & LinkedIn.
- **Tentang Singkat**: ringkasan profil dan minat.
- **Skill / Minat**: kartu/ikon — Linux, Jaringan, Web, Mobile, IoT.
- **Pendidikan**: timeline (SMKS Nuris Jember 2018–2021; D3 Teknik Komputer Polije).
- **Project Unggulan**: 3 proyek terbaru/terpilih.
- **Blog Terbaru**: 3 artikel terbaru.
- **CTA Kontak** di bagian bawah.

#### B. Blog (`/blog` dan `/blog/[slug]`)
- Daftar artikel dalam bentuk kartu (thumbnail, judul, ringkasan, tanggal, kategori/tag).
- Pencarian dan filter kategori/tag.
- Pagination.
- Halaman detail: konten Markdown, estimasi waktu baca, tanggal terbit, artikel terkait, tombol bagikan.
- Meta SEO dan Open Graph per artikel.

#### C. Project (`/project` dan `/project/[slug]`)
- Daftar proyek dalam grid kartu (gambar, judul, deskripsi singkat, tech stack).
- Filter berdasarkan kategori (Web, Mobile, IoT, Jaringan, Lainnya).
- Halaman detail: deskripsi lengkap, galeri gambar, tech stack, tautan demo & repository GitHub, peran, tanggal.

#### D. Kontak (`/kontak`)
- Form: nama, email, subjek, pesan.
- Validasi sisi klien dan server; proteksi spam (honeypot + rate limit).
- Notifikasi sukses/gagal.
- Informasi kontak dan tautan GitHub & LinkedIn.

### 8.2 Chat AI Mengambang (Seluruh Halaman Publik)
- Tombol bulat mengambang di pojok kanan bawah pada semua halaman publik.
- Klik membuka jendela chat (panel/popup), dapat diminimalkan dan ditutup.
- Fitur:
  - Input teks, kirim dengan Enter.
  - Respons AI (streaming bila memungkinkan) dengan indikator mengetik.
  - Riwayat percakapan disimpan di sesi browser (state/sessionStorage).
  - Tombol "Mulai percakapan baru".
  - Saran pertanyaan cepat (mis. "Apa saja skill Ridho?", "Ceritakan project IoT-nya").
  - Dukungan render Markdown pada jawaban.
- Perilaku AI:
  - *System prompt* berisi konteks profil, pendidikan, minat, proyek, dan blog Ridho (diambil dari database).
  - Menjawab dalam bahasa yang digunakan pengguna (default Bahasa Indonesia).
  - Menolak topik di luar konteks portfolio secara sopan.
  - Tidak mengarang informasi yang tidak ada; menyarankan form kontak bila tidak tahu.
- Pengamanan: rate limit per IP, batas panjang pesan, batas jumlah riwayat yang dikirim, sanitasi input.
- Tampil adaptif pada dark mode dan mobile (layar penuh pada layar kecil).

### 8.3 Dark Mode
- Toggle tema (Light / Dark / System) di navbar.
- Preferensi tersimpan (localStorage) dan mengikuti preferensi sistem secara default.
- Tanpa *flash* tema saat load (menggunakan `next-themes` dan class `dark` Tailwind).
- Seluruh komponen termasuk chat AI dan panel admin mendukung dark mode.

### 8.4 Panel Admin (`/admin`)
- **Login** (email + password) dengan Laravel Sanctum.
- **Dashboard**: ringkasan jumlah blog, project, dan pesan belum dibaca.
- **Kelola Blog**: daftar, tambah, ubah, hapus; status Draft/Published; upload thumbnail; editor Markdown dengan pratinjau; kategori & tag.
- **Kelola Project**: daftar, tambah, ubah, hapus; upload gambar (banyak); tech stack; tautan demo/repo; flag "unggulan".
- **Kelola Profil**: edit bio, headline, foto, skill, pendidikan, tautan sosial.
- **Pesan Masuk**: daftar pesan dari form kontak, tandai dibaca, hapus.
- **Pengaturan AI** (opsional): edit tambahan instruksi/konteks untuk chat AI.
- Seluruh rute admin terlindungi middleware autentikasi; logout dan expired token ditangani.

## 9. Desain UI/UX

### Prinsip
Bersih, modern, minimalis, fokus pada konten, dan konsisten.

### Panduan Visual
- **Tipografi**: font sans-serif modern (mis. Inter / Geist) via `next/font`.
- **Warna**: palet netral dengan satu warna aksen (mis. indigo/violet); kontras memenuhi WCAG AA.
- **Komponen**: kartu dengan sudut membulat (`rounded-2xl`), bayangan lembut, border tipis.
- **Efek**: *glassmorphism* ringan pada navbar, gradient halus pada hero, animasi *fade/slide* saat scroll (hormati `prefers-reduced-motion`).
- **Layout**: container maksimum ±1200px, grid responsif, navbar sticky.
- **Navigasi**: Beranda · Blog · Project · Kontak + toggle tema.
- **Responsif**: mobile-first; navbar menjadi menu hamburger pada layar kecil.
- **Aksesibilitas**: HTML semantik, label form, fokus terlihat, navigasi keyboard, `alt` pada gambar.

## 10. Model Data (Database)

| Tabel | Kolom Utama |
|---|---|
| **users** | id, name, email, password, timestamps |
| **profiles** | id, name, headline, bio, photo, location, github_url, linkedin_url, email, resume_url |
| **skills** | id, name, category, icon, level, order |
| **educations** | id, institution, major, start_year, end_year, description, order |
| **projects** | id, title, slug, summary, description, thumbnail, category, tech_stack (JSON), demo_url, repo_url, is_featured, status, timestamps |
| **project_images** | id, project_id, path, caption, order |
| **posts** | id, title, slug, excerpt, content (markdown), thumbnail, category_id, status (draft/published), published_at, timestamps |
| **categories** | id, name, slug |
| **tags** | id, name, slug |
| **post_tag** | post_id, tag_id |
| **messages** | id, name, email, subject, body, is_read, ip_address, timestamps |
| **settings** | id, key, value (misal konteks tambahan AI) |

Seed awal: data pendidikan (SMKS Nuris Jember 2018–2021 dan D3 Teknik Komputer Polije), skill/minat, tautan sosial, dan satu akun admin.

## 11. Spesifikasi API (Laravel)

Base URL: `/api/v1`

### Publik
| Method | Endpoint | Deskripsi |
|---|---|---|
| GET | `/profile` | Profil, skill, pendidikan, tautan sosial |
| GET | `/projects` | Daftar project (filter `category`, `featured`, pagination) |
| GET | `/projects/{slug}` | Detail project |
| GET | `/posts` | Daftar blog published (`search`, `category`, `tag`, pagination) |
| GET | `/posts/{slug}` | Detail blog |
| GET | `/categories` | Daftar kategori |
| POST | `/contact` | Kirim pesan kontak (rate limited) |
| POST | `/chat` | Kirim pesan ke AI, mengembalikan jawaban (atau stream) |

### Admin (butuh `Authorization: Bearer <token>`)
| Method | Endpoint | Deskripsi |
|---|---|---|
| POST | `/admin/login` | Login admin |
| POST | `/admin/logout` | Logout |
| GET | `/admin/me` | Data admin saat ini |
| GET | `/admin/dashboard` | Statistik ringkas |
| CRUD | `/admin/posts` | Kelola blog |
| CRUD | `/admin/projects` | Kelola project |
| CRUD | `/admin/categories` & `/admin/tags` | Kelola kategori & tag |
| PUT | `/admin/profile` | Ubah profil |
| CRUD | `/admin/skills` & `/admin/educations` | Kelola skill & pendidikan |
| GET/PATCH/DELETE | `/admin/messages` | Kelola pesan masuk |
| POST | `/admin/upload` | Upload gambar |
| GET/PUT | `/admin/settings` | Pengaturan (konteks AI) |

Format respons konsisten:
```json
{
  "success": true,
  "message": "OK",
  "data": {},
  "meta": { "page": 1, "per_page": 10, "total": 25 }
}
```

## 12. Kebutuhan Non-Fungsional

| Aspek | Target |
|---|---|
| **Performa** | LCP < 2,5 dtk; gambar dioptimasi (`next/image`, WebP); caching respons API |
| **SEO** | Meta tag dinamis, Open Graph, `sitemap.xml`, `robots.txt`, URL slug bersih |
| **Keamanan** | HTTPS, Sanctum, validasi input, proteksi CSRF/XSS/SQL injection, rate limiting, CORS terbatas, API key hanya di server, password di-hash (bcrypt/argon) |
| **Aksesibilitas** | Standar WCAG 2.1 AA |
| **Responsif** | Mendukung ≥ 320px hingga layar lebar |
| **Kompatibilitas** | Browser modern (Chrome, Firefox, Safari, Edge — 2 versi terakhir) |
| **Maintainability** | TypeScript strict, ESLint + Prettier, Laravel Pint, struktur modular |
| **Keandalan** | Penanganan error yang ramah (halaman 404/500, fallback chat AI bila API gagal) |

## 13. Struktur Proyek

```
portfolio/
├── frontend/                  # Next.js + TypeScript + Tailwind
│   ├── src/
│   │   ├── app/
│   │   │   ├── (public)/
│   │   │   │   ├── page.tsx            # Beranda
│   │   │   │   ├── blog/
│   │   │   │   ├── project/
│   │   │   │   └── kontak/
│   │   │   ├── admin/                  # Panel admin
│   │   │   ├── layout.tsx
│   │   │   └── globals.css
│   │   ├── components/
│   │   │   ├── ui/                     # Button, Card, Input, dll.
│   │   │   ├── layout/                 # Navbar, Footer, ThemeToggle
│   │   │   ├── chat/                   # FloatingChat, ChatWindow, MessageBubble
│   │   │   └── sections/               # Hero, Education, Skills, dll.
│   │   ├── lib/                        # api client, utils
│   │   ├── types/                      # Tipe TypeScript
│   │   └── hooks/
│   ├── public/
│   ├── tailwind.config.ts
│   └── package.json
│
└── backend/                   # Laravel API
    ├── app/
    │   ├── Http/Controllers/Api/       # Public & Admin controllers
    │   ├── Http/Requests/
    │   ├── Http/Resources/
    │   ├── Models/
    │   └── Services/AiChatService.php
    ├── database/{migrations,seeders}/
    ├── routes/api.php
    └── .env
```

## 14. Alur Pengguna Utama

1. **Pengunjung → Kontak**: Buka `/kontak` → isi form → validasi → pesan tersimpan → muncul di panel admin.
2. **Pengunjung → Chat AI**: Klik tombol mengambang → ketik pertanyaan → frontend memanggil `POST /api/chat` → Laravel menyusun *system prompt* + riwayat → memanggil Claude API → jawaban ditampilkan.
3. **Admin → Publikasi Blog**: Login → Kelola Blog → Tambah → tulis Markdown + thumbnail → set status *Published* → muncul di `/blog`.
4. **Admin → Tambah Project**: Login → Kelola Project → isi detail + gambar → tampil di `/project` dan beranda (bila unggulan).

## 15. Rencana Pengembangan (Milestone)

| Fase | Cakupan | Estimasi |
|---|---|---|
| **1. Setup** | Inisialisasi repo, Next.js + Tailwind + TS, Laravel, database, CORS, Sanctum | 2–3 hari |
| **2. Backend Inti** | Migration, model, seeder, endpoint publik (profile, projects, posts, contact) | 4–5 hari |
| **3. Frontend Publik** | Layout, navbar/footer, dark mode, Beranda, Blog, Project, Kontak | 5–7 hari |
| **4. Panel Admin** | Auth, dashboard, CRUD blog/project/profil, pesan masuk, upload | 5–7 hari |
| **5. Chat AI** | Endpoint `/chat`, service Claude, widget mengambang, rate limit | 3–4 hari |
| **6. Polish & QA** | SEO, aksesibilitas, optimasi performa, testing, perbaikan bug | 3–4 hari |
| **7. Deploy** | Konfigurasi server/hosting, domain, SSL, CI/CD sederhana | 2 hari |

## 16. Kriteria Penerimaan (Acceptance Criteria)

- [ ] Empat halaman publik (Beranda, Blog, Project, Kontak) tampil benar dan responsif.
- [ ] Data pendidikan dan tautan GitHub/LinkedIn tampil sesuai.
- [ ] Dark mode berfungsi di semua halaman tanpa *flicker* dan tersimpan setelah refresh.
- [ ] Tombol chat AI muncul di seluruh halaman publik dan dapat memberi jawaban kontekstual.
- [ ] Form kontak menyimpan pesan dan menampilkan umpan balik yang jelas.
- [ ] Admin dapat login, mengelola blog, project, profil, dan membaca pesan.
- [ ] Rute admin tidak dapat diakses tanpa autentikasi.
- [ ] API key AI tidak pernah terlihat di sisi klien.
- [ ] Lighthouse ≥ 90 pada halaman utama.
- [ ] Tidak ada error TypeScript/ESLint pada build produksi.

## 17. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|---|---|---|
| Biaya/penyalahgunaan API AI | Tagihan membengkak | Rate limit, batas panjang pesan, batas token, monitoring |
| Spam pada form kontak & chat | Kotak masuk penuh | Honeypot, throttle, validasi |
| Kebocoran kredensial | Keamanan | `.env` tidak masuk repo, token Sanctum, HTTPS |
| Jawaban AI tidak akurat | Informasi keliru | *System prompt* ketat, konteks dari database, fallback ke form kontak |
| Cakupan melebar | Jadwal molor | Patuhi *scope* v1.0, fitur tambahan masuk backlog |

## 18. Pengembangan Lanjutan (Backlog)

- Komentar & reaksi pada blog.
- Multi-bahasa (ID/EN).
- Unduh CV (PDF) otomatis dari data profil.
- Statistik pengunjung (analytics) sederhana.
- RSS feed blog.
- Newsletter.
- Dukungan PWA.
- Mode "suara" pada chat AI.

---

*Dokumen ini adalah draf awal dan dapat direvisi sesuai kebutuhan selama pengembangan.*
