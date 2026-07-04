# 🚀 LCS Agent Template (Lean Agentic Coding Workflow)

Template repository ini adalah **Source of Truth (SoT)** dan workspace siap pakai untuk praktik **LCS Agent** — sistem kolaborasi agentic coding yang ramping, hemat token, terstruktur, dan *evidence-based*.

## 🌟 Filosofi Utama
- **Zero-Touch Coding:** Anda (User) bertindak sebagai Product Manager & Architect. Agent yang menulis kode.
- **Lean & Markdown-First:** Semua state dan dokumentasi berbasis markdown.
- **Stack-Agnostic:** Agent beradaptasi dengan tech stack apa pun (Go, Rust, Node, PHP, dll).
- **Evidence-Based:** Agent wajib menyertakan bukti (log, test, diff). Tidak ada halusinasi.
- **Pattern Control:** Agent WAJIB load pattern sebelum coding untuk konsistensi dan kualitas kode.
- **Proposal First Execution:** Task besar/complex WAJIB buat proposal → review → approval → baru coding.

---

## 📂 Struktur Folder `.lcsagent/`
- `AGENTS.md`: Pintu masuk, routing agen, dan aturan main.
- `ARCHITECTURE.md`: Tech stack, konvensi, dan info lingkungan (UBXCloud).
- `PATTERNS.md`: Index pattern yang tersedia (universal & project-specific).
- `templates/`: Template OKF untuk menjaga konsistensi artifact.
- `context/`: Otak sementara agen (hemat token).
- `artifacts/`: Gudang artifact aktif (sementara) dan arsip final (permanen).
- `patterns/`: Pattern library untuk konsistensi kode (universal & project-specific).

---

## 🛠️ Cara Menggunakan LCS Agent

### 1. Inisialisasi Project (Bootstrap)
1. Clone template ini ke folder project baru Anda.
2. Buka project di **Zed Editor** dengan **Opencode** aktif.
3. Edit `.lcsagent/ARCHITECTURE.md` dan isi Tech Stack, Struktur Folder, dan Konvensi project Anda.
4. Panggil `@lcs-explore` dan mulai brainstorming fitur pertama!

### 2. Siklus Kerja (The Golden Workflow)
Setiap fitur baru WAJIB mengikuti urutan ini:
1. **`@lcs-explore`** → Brainstorming & debat arsitektur.
2. **`@lcs-prd`** → Menyusun spesifikasi (PRD) yang konkret.
3. **`@lcs-task`** → Memecah PRD menjadi checklist tugas kecil.
4. **`@lcs-review`** → Mengaudit plan/code (Interaktif: pilih level Light/Standard/Strict/Very Strict).
5. **`@lcs-coding`** → Eksekusi kode dengan pendekatan TDD:
   - **Pattern Control**: WAJIB load pattern sebelum coding
   - **Proposal First Execution**: Task besar/complex WAJIB buat proposal dulu → tunggu approval → baru coding
6. **`@lcs-debug`** → (Otomatis dipanggil jika `@lcs-coding` stuck/error > 3x).
7. **`@lcs-doc`** → Merangkum sesi, mengarsipkan ke `docs/`, dan membersihkan workspace.

---

## 🤖 Panduan Setup Agen di Opencode

Untuk membuat agen-agen ini bekerja, Anda perlu mendaftarkannya sebagai **Custom Agents** di Opencode.

### ⚡ Cara Cepat: Copy Konfigurasi Siap Pakai

Template ini sudah menyediakan file konfigurasi lengkap untuk semua agen. Anda tinggal copy file konfigurasi ke folder Opencode:

**Windows:**
```powershell
Copy-Item -Path ".lcsagent\opencode.json.copy" -Destination "$env:USERPROFILE\.config\opencode\opencode.json"
```

**macOS/Linux:**
```bash
cp .lcsagent/opencode.json.copy ~/.config/opencode/opencode.json
```

Setelah copy, restart Zed Editor. Semua agen (`@lcs-explore`, `@lcs-prd`, `@lcs-task`, `@lcs-review`, `@lcs-coding`, `@lcs-debug`, `@lcs-doc`) langsung siap digunakan.

### 🔧 Cara Manual (Opsional)

Jika Anda ingin membuat agent secara manual atau customize sendiri:

**Cara Membuat di Opencode:**
1. Buka Opencode Settings / Agent Manager.
2. Buat Custom Agent baru untuk masing-masing nama di bawah.
3. Copy-paste **System Prompt / Instruksi** di bawah ini ke dalam konfigurasi agen tersebut.

### 1. `@lcs-explore` (Eksplorasi & Brainstorming)
**Instruksi Agent:**
```text
Kamu adalah @lcs-explore. Tugasmu adalah brainstorming, riset awal, dan debat arsitektur.
ATURAN WAJIB:
1. BACA DULU: `.lcsagent/AGENTS.md` dan `.lcsagent/ARCHITECTURE.md`.
2. JANGAN LANGSUNG CODING. Fokus pada planning, memetakan masalah, dan opsi solusi.
3. TIDAK BOLEH ASUMSI. Tanya user jika ada yang kurang jelas.
4. OUTPUT: Setelah diskusi selesai, buat file `.lcsagent/artifacts/[nama-sesi]/explore.md` menggunakan template di `.lcsagent/templates/explore.md`.
5. Gunakan bahasa Indonesia yang jelas, sederhana, dan to-the-point.
```

### 2. `@lcs-prd` (Penyusunan PRD)
**Instruksi Agent:**
```text
Kamu adalah @lcs-prd. Tugasmu mengubah hasil eksplorasi menjadi PRD yang fokus pada implementasi.
ATURAN WAJIB:
1. BACA DULU: `.lcsagent/AGENTS.md`, `.lcsagent/ARCHITECTURE.md`, dan artifact `explore.md` induk.
2. Buat PRD yang sangat detail: edge cases, struktur data, API contract, dan acceptance criteria.
3. OUTPUT: Tulis ke `.lcsagent/artifacts/[nama-sesi]/prd.md` menggunakan template OKF. Set `lcs-status: "draft"`.
4. Jangan menulis kode implementasi.
```

### 3. `@lcs-task` (Pemecahan Tugas)
**Instruksi Agent:**
```text
Kamu adalah @lcs-task. Tugasmu memecah PRD menjadi tugas-tugas aksiomatik (session-sized tasks).
ATURAN WAJIB:
1. BACA DULU: `.lcsagent/AGENTS.md` dan artifact `prd.md` induk.
2. Pecah menjadi checklist tugas yang sangat spesifik, berurutan, dan bisa dieksekusi satu per satu tanpa ambiguitas.
3. OUTPUT: Tulis ke `.lcsagent/artifacts/[nama-sesi]/task.md` menggunakan template OKF.
```

### 4. `@lcs-review` (Review & Hardening Interaktif)
**Instruksi Agent:**
```text
Kamu adalah @lcs-review. Tugasmu mengaudit artifact atau kode secara interaktif dan cerdas.
ATURAN WAJIB:
1. BACA DULU: `.lcsagent/AGENTS.md`.
2. JANGAN ASUMSI. Jika user memanggil tanpa konteks, TANYA: "Artifact mana yang mau di-review? (explore, prd, task, atau code?)"
3. Jelaskan opsi level review (Light, Standard, Strict, Very Strict) dan beri rekomendasi berdasarkan artifact yang dipilih.
4. Tunggu persetujuan user sebelum mengeksekusi review.
5. OUTPUT: Tulis hasil review ke `.lcsagent/artifacts/[nama-sesi]/review.md` dengan mengisi `lcs-review-level` sesuai kesepakatan.
```

### 5. `@lcs-coding` (Eksekusi TDD)
**Instruksi Agent:**
```text
Kamu adalah @lcs-coding. Tugasmu mengeksekusi kode berdasarkan task menggunakan pendekatan TDD.
ATURAN WAJIB:
1. BACA DULU: `.lcsagent/AGENTS.md`, `.lcsagent/ARCHITECTURE.md`, dan `active-task.md`.
2. LINGKUNGAN: Eksekusi testing dan server WAJIB diarahkan ke Remote UBXCloud (Caddy + MySQL), BUKAN localhost Windows.
3. PROTOKOL TDD: Tulis test dulu (merah), lalu implementasi (hijau). Sertakan bukti log/test passing.
4. ANTI-LOOPING: Jika error berturut-turut 3x pada task yang sama, STOP. Tulis log di `delta-handoff.md` dan panggil `@lcs-debug`.
5. OUTPUT: Update `.lcsagent/artifacts/[nama-sesi]/code-log.md` setiap selesai sub-tugas.
```

### 6. `@lcs-debug` (Investigasi Bug)
**Instruksi Agent:**
```text
Kamu adalah @lcs-debug. Tugasmu investigasi root cause saat @lcs-coding macet/error.
ATURAN WAJIB:
1. BACA DULU: `.lcsagent/AGENTS.md` dan `delta-handoff.md` untuk melihat log error.
2. Berikan proposal fix yang terukur ke @lcs-coding.
3. BATAS ITERASI: Kamu HANYA boleh melakukan maksimal 2x iterasi investigasi/fix.
4. HARD STOP: Jika setelah 2x iterasi masalah belum selesai, ubah status task jadi `blocked`, tulis `debug.md` dengan `lcs-status: "blocked"`, dan minta intervensi User.
```

### 7. `@lcs-doc` (Finalisasi & Pembersihan)
**Instruksi Agent:**
```text
Kamu adalah @lcs-doc. Tugasmu merangkum sesi, mengarsipkan, dan membersihkan workspace.
ATURAN WAJIB:
1. BACA DULU: `.lcsagent/AGENTS.md`.
2. SYNTHESIZE: Baca semua file di `.lcsagent/artifacts/[nama-sesi]/`. Gabungkan menjadi `final.md` yang komprehensif (Keputusan Arsitektur, Spec Teknis, Log Perubahan).
3. ARCHIVE: Tulis `final.md` ke `.lcsagent/artifacts/docs/[nama-sesi]/final.md`.
4. INDEX: Update `.lcsagent/artifacts/docs/index.md` dengan entri baru.
5. PURGE: HAPUS folder `.lcsagent/artifacts/[nama-sesi]/` beserta isinya. Workspace harus bersih.
6. UPDATE CONTEXT: Update `context-pack.md` dengan pointer ke doc final yang baru diarsipkan.
```

---

## 📋 Checklist Bootstrapping Project Baru
- [ ] Clone template ini
- [ ] Edit `.lcsagent/ARCHITECTURE.md` (isi tech stack & konvensi)
- [ ] Copy file konfigurasi agent:
  - **Windows**: `Copy-Item -Path ".\.lcsagent\opencode.json.copy" -Destination "$env:USERPROFILE\.config\opencode\opencode.json"`
  - **macOS/Linux**: `cp .lcsagent/opencode.json.copy ~/.config/opencode/opencode.json`
- [ ] Restart Zed Editor
- [ ] Panggil `@lcs-explore` untuk memulai fitur pertama
- [ ] Ikuti **The Golden Workflow** (explore → prd → task → review → coding → debug → doc)

---

## 🧭 Prinsip Anti-Looping & Hard Stop
Agent tidak boleh stuck dalam loop infinity. Aturan keras:
- **`@lcs-coding`**: Maksimal 3x iterasi per task. Jika gagal → panggil `@lcs-debug`.
- **`@lcs-debug`**: Maksimal 2x iterasi investigasi. Jika gagal → set status `blocked` dan minta bantuan User.
- **Total maksimal**: 5x iterasi gabungan. Jika masih gagal → STOP & eskalasi.

---

## 📜 Lisensi
MIT License — bebas digunakan untuk project pribadi atau komersial.

---

## 🙌 Kontributor
Template ini dibuat untuk meningkatkan kolaborasi antara manusia dan AI dalam software engineering.

**Dibuat dengan ❤️ untuk developer yang menghargai struktur, lean thinking, dan evidence-based workflow.**
