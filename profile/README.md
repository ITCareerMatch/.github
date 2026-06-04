# 💼 ITCareerMatch
### AI Career Matching & Skill Gap Analysis Platform

[![React](https://img.shields.io/badge/React-18.2+-61DAFB?style=flat-square&logo=react&logoColor=white)](https://reactjs.org/)
[![Vite](https://img.shields.io/badge/Vite-5.0+-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-4.0+-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Express.js](https://img.shields.io/badge/Express.js-4+-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2+-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)](https://www.tensorflow.org/)
[![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white)](https://supabase.com/)
[![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)](https://vercel.com/)

> Platform berbasis AI yang membantu pencari kerja IT menemukan lowongan yang paling sesuai dengan CV mereka, dilengkapi analisis skill gap dan chatbot konsultasi karir.

🌐 **Live Demo**: [https://itcareermatch.vercel.app/](https://itcareermatch.vercel.app/)
📄 **Dokumentasi API**: [https://itcareermatch.up.railway.app/api-docs/](https://itcareermatch.up.railway.app/api-docs/)
📊 **Dashboard**: [https://dashboard-itcareermatch.streamlit.app/](https://dashboard-itcareermatch.streamlit.app/)
📁 **Repository**: [https://github.com/ITCareerMatch](https://github.com/ITCareerMatch)

---

## 📋 Ringkasan Proyek

**ITCareerMatch** hadir sebagai solusi atas kesenjangan antara pencari kerja IT dan kebutuhan industri. Pengguna cukup mengunggah CV mereka, lalu sistem akan:

- **Mengekstrak skill** secara otomatis dari CV
- **Menghitung match score** dengan berbagai posisi IT (dalam persentase)
- **Menampilkan skill gap** per posisi lengkap dengan insight AI
- **Merekomendasikan lowongan** yang paling relevan
- **Menyediakan chatbot** untuk konsultasi karir berbasis profil CV

---

## 🏗️ Arsitektur Sistem

```
┌─────────────────────────────────────────────────────┐
│              Frontend (React + Vite)                 │
│            https://itcareermatch.vercel.app          │
└──────────────────────┬──────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────┐
│         Express.js Backend (Node.js)                 │
│   Auth · CV Parsing · Queue · API Gateway            │
└──────┬──────────────────────────┬───────────────────┘
       │                          │
┌──────▼──────────┐    ┌──────────▼──────────────────┐
│  FastAPI SBERT  │    │     FastAPI Chatbot           │
│  Job Matching   │    │   Groq (Llama 3.3 70B) + TTS │
└──────┬──────────┘    └──────────┬───────────────────┘
       │                          │
┌──────▼──────────────────────────▼───────────────────┐
│        Supabase (PostgreSQL + Storage)               │
│      Redis (BullMQ queue + session sementara)        │
└─────────────────────────────────────────────────────┘
```

---

## 📁 Struktur Repository

```
ITCareerMatch/
├── Frontend-ITCareerMatch/       # React + Vite frontend
├── Backend-ITCareerMatch/        # Express.js backend + worker
├── AI-ITCareerMatch/
│   ├── sbert/                    # Model SBERT (TensorFlow + HuggingFace)
│   └── chatbot/                  # FastAPI chatbot & TTS
└── Data-Science-ITCareerMatch/   # Pipeline data science & EDA
```

Dokumentasi detail tiap komponen tersedia di folder masing-masing:
- [Frontend README](https://github.com/ITCareerMatch/Frontend-ITCarrerMatch)
- [Backend README](https://github.com/ITCareerMatch/Backend-ITCareerMatch)
- [AI README](https://github.com/ITCareerMatch/AI-Engineer-ITCareerMatch)
- [Data Science README](https://github.com/ITCareerMatch/Data-Science-ITCareerMatch)

---

## ⚙️ Prasyarat

Pastikan tools berikut sudah terinstal sebelum memulai:

| Tool | Versi Minimum | Kegunaan |
|------|--------------|----------|
| [Node.js](https://nodejs.org/) | 18+ | Frontend & Backend |
| [Python](https://python.org/) | 3.9+ | AI Services & Data Science |
| [Redis](https://redis.io/) | 6+ | Queue & Session |
| [npm](https://npmjs.com/) | 9+ | Package manager JS |
| [pip](https://pip.pypa.io/) | 23+ | Package manager Python |

Akun & layanan yang diperlukan:
- [Supabase](https://supabase.com/) — database & storage
- [Groq](https://console.groq.com/) — LLM API key
- [Vercel](https://vercel.com/) — hosting frontend *(opsional untuk lokal)*

---

## 🚀 Menjalankan Proyek Secara Lokal

Karena proyek terdiri dari beberapa layanan, jalankan masing-masing di terminal terpisah.

### 1. Clone Repository

```bash
git clone https://github.com/ITCareerMatch/ITCareerMatch.git
cd ITCareerMatch
```

### 2. Setup Frontend

```bash
cd Frontend-ITCareerMatch
npm install
cp .env.example .env
# Edit .env: isi VITE_API_BASE_URL, VITE_SUPABASE_URL, VITE_SUPABASE_ANON_KEY
npm run dev
# Berjalan di http://localhost:5173
```

### 3. Setup Backend

```bash
cd Backend-ITCareerMatch
npm install
cp .env.example .env
# Edit .env: isi SUPABASE_URL, SUPABASE_SERVICE_ROLE_KEY, REDIS_URL, GROQ_API_KEY, dll.
npm run migrate       # Jalankan migrasi database
npm run dev           # Terminal 1: API server di http://localhost:3000
npm run worker        # Terminal 2: BullMQ worker
```

### 4. Setup AI Service (SBERT + Chatbot)

```bash
cd AI-ITCareerMatch/chatbot
pip install -r requirements.txt
npm install
cp .env.example .env
# Edit .env: isi GROQ_API_KEY, SUPABASE_URL, SUPABASE_SERVICE_ROLE_KEY
uvicorn main:app --reload --port 8000   # Terminal 3: FastAPI
```

### 5. (Opsional) Jalankan Data Science Dashboard

```bash
pip install streamlit pandas numpy plotly
streamlit run dashboard.py
# Atau akses langsung: https://dashboard-itcareermatch.streamlit.app/
```

---

## 🔑 Environment Variables

### Backend (`.env`)

| Variable | Keterangan |
|----------|------------|
| `PORT` | Port server (default: `3000`) |
| `DATABASE_URL` | URL koneksi PostgreSQL Supabase |
| `SUPABASE_URL` | URL project Supabase |
| `SUPABASE_SERVICE_ROLE_KEY` | Service role key Supabase |
| `REDIS_URL` | URL Redis (default: `redis://localhost:6379`) |
| `AI_API_URL` | URL FastAPI SBERT service |
| `GROQ_API_KEY` | API key dari [console.groq.com](https://console.groq.com) |
| `INTERNAL_API_KEY` | Secret key komunikasi internal |

### Frontend (`.env`)

| Variable | Keterangan |
|----------|------------|
| `VITE_API_BASE_URL` | URL backend API |
| `VITE_SUPABASE_URL` | URL project Supabase |
| `VITE_SUPABASE_ANON_KEY` | Supabase anonymous key |

---

## 🔄 Alur Penggunaan Aplikasi

```
1. Upload CV (PDF)
        ↓
2. Backend parsing & ekstraksi skill otomatis
        ↓
3. SBERT menghitung match score CV ↔ semua lowongan
        ↓
4. Tampilkan rekomendasi lowongan + skill gap per posisi
        ↓
5. User konsultasi karir via Chatbot (konteks CV otomatis diinjek)
```

---

## 🛠️ Tech Stack Lengkap

| Layer | Teknologi |
|-------|-----------|
| Frontend | React.js 18, Vite, Tailwind CSS 4, React Router DOM v7, Framer Motion |
| Backend | Node.js 18+, Express.js, BullMQ, Redis, Supabase Auth |
| AI — Job Matching | Fine-tuned SBERT (TensorFlow + HuggingFace), FastAPI |
| AI — Chatbot & TTS | Llama 3.3 70B via Groq API, Orpheus TTS via Groq |
| Database & Storage | Supabase (PostgreSQL + Storage) |
| Data Science | Python, Pandas, Selenium (scraping), Streamlit, Groq LLM (normalisasi) |
| Deployment | Vercel (frontend), Streamlit Cloud (dashboard) |

---

## 👥 Tim Pengembang — CC26-PSU088

| Foto | Nama | Learning Path | Peran |
|------|------|--------------|-------|
| <img src="https://raw.githubusercontent.com/ITCareerMatch/.github/main/Chardinal.png" width="50" style="aspect-ratio:1/1; object-fit:cover"> | [Chardinal Martin Butarbutar](https://github.com/chardinal) | Data Science | Data pipeline, EDA, preprocessing |
| <img src="https://raw.githubusercontent.com/ITCareerMatch/.github/main/Della.jpg" width="50" style="aspect-ratio:1/1; object-fit:cover"> | [Nadhia Della Puspita Sari](https://github.com/NadhiaDella) | Data Science | Data pipeline, EDA, preprocessing |
| <img src="https://raw.githubusercontent.com/ITCareerMatch/.github/main/Angel.jpg" width="50" style="aspect-ratio:1/1; object-fit:cover"> | [Mutiara Angelita Muhaeni](https://github.com/kaenjie) | Fullstack Dev | Frontend React, UI/UX |
| <img src="https://raw.githubusercontent.com/ITCareerMatch/.github/main/Asep.jpg" width="50" style="aspect-ratio:1/1; object-fit:cover"> | [Ahmad Sefriadi](https://github.com/sefriadiahmad) | Fullstack Dev | Backend Express, API, integrasi |
| <img src="https://raw.githubusercontent.com/ITCareerMatch/.github/main/Abil.webp" width="50" style="aspect-ratio:1/1; object-fit:cover"> | [Muhammad Arifbillah Kamil](https://github.com/ArifbillahKamil) | AI Engineer | SBERT model, FastAPI |
| <img src="https://raw.githubusercontent.com/ITCareerMatch/.github/main/Ulil.jpeg" width="50" style="aspect-ratio:1/1; object-fit:cover"> | [Ulil Noor Absor](https://github.com/ulillearn) | AI Engineer | Chatbot service, TTS |

---

## 📄 Lisensi

Proyek ini merupakan bagian dari **Capstone Project Coding Camp 2026 powered by DBS Foundation** — Tim CC26-PSU088.

MIT License — lihat file [LICENSE](LICENSE) untuk detail lebih lanjut.

---

<p align="center">
  <strong>Dibuat dengan ❤️ untuk komunitas IT Indonesia</strong><br>
  <sub>Coding Camp 2026 × DBS Foundation</sub>
</p>
