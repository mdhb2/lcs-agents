# 🚪 LCS Agent: Pintu Masuk & Routing
**Status:** Active | **Versi:** 2.0

Dokumen ini adalah "Kitab Suci" untuk workflow. Setiap agen WAJIB membaca file ini pertama kali.

## 📌 Aturan Emas (Rules of Engagement)
1. **Baca Dulu:** WAJIB baca `AGENTS.md` ini dan `.lcsagent/ARCHITECTURE.md` sebelum bekerja.
2. **Zero-Touch & No Assumption:** User tidak menulis kode. Jangan berasumsi, TANYA jika ragu.
3. **Jangan Langsung Coding:** Fokus planning/brainstorming sampai PRD disepakati.
4. **Hemat Token (Compact Context):** Baca `context/context-pack.md` & `active-task.md`. Jangan baca semua file.
5. **Ikuti Template:** WAJIB gunakan template di `.lcsagent/templates/` untuk menulis artifact.
6. **Chain of Truth:** Sertakan bukti (log, test, diff). No silent failures.
7. **Anti-Looping:** `@lcs-coding` max 3x iterasi. Jika gagal, panggil `@lcs-debug` (max 2x). Total max 5x. Jika masih gagal → STOP & set `lcs-status: "blocked"`.

## 🗺️ Routing Agen & Trigger Artifact
| Agen | Tugas Utama | Output Artifact |
| :--- | :--- | :--- |
| `@lcs-explore` | Brainstorming, debat arsitektur | `artifacts/[session]/explore.md` |
| `@lcs-prd` | Susun spesifikasi konkret | `artifacts/[session]/prd.md` |
| `@lcs-task` | Pecah PRD jadi checklist tugas | `artifacts/[session]/task.md` |
| `@lcs-review` | Audit plan/code (Interaktif) | `artifacts/[session]/review.md` |
| `@lcs-coding` | Eksekusi TDD | Update `artifacts/[session]/code-log.md` |
| `@lcs-debug` | Investigasi bug (saat coding macet) | `artifacts/[session]/debug.md` |
| `@lcs-doc` | Rangkum sesi, arsip, bersihkan workspace | `artifacts/docs/[session]/final.md` |

## 📝 Artifact Contract (Hukum Pengikat)
- **Parent-Child:** Tidak boleh buat artifact baru jika `lcs-parent` belum `final`.
- **Traceability:** Setiap artifact WAJIB isi `lcs-parent`.
- **Format:** Gunakan OKF + LCS Contract Metadata (lihat folder `templates/`).

---

## 🔧 Workflow Detail: @lcs-coding dengan Pattern Control & Proposal First

### Step 1: Analisis Task
- Baca `context/active-task.md`
- Tentukan apakah task butuh proposal:
  - **WAJIB Proposal**: Task besar (multi-file, >3 file), complex (banyak edge case), kritikal (DB schema, security), fitur baru
  - **TIDAK PERLU Proposal**: Task kecil (1-2 file, bug fix), simple, update dokumentasi, tweak UI minor
  - **Override**: User bisa explicitly bilang "Pakai proposal" atau "Langsung kerjakan"

### Step 2: Load Pattern
- Baca `PATTERNS.md` untuk cek pattern yang relevan
- Load pattern spesifik berdasarkan keyword di task:
  - "API" / "endpoint" → `patterns/project-specific/api-handler.md`
  - "error" / "exception" → `patterns/project-specific/error-handling.md`
  - "database" / "query" → `patterns/project-specific/database-query.md`
  - "test" / "TDD" → `patterns/universal/tdd-workflow.md`
  - Task besar / complex → `patterns/universal/proposal-first.md`
- Catat pattern yang akan dipakai

### Step 3: Buat Proposal (Jika Butuh)
- Copy template dari `templates/proposal.md`
- Simpan di `context/proposal.md`
- Isi semua section:
  - Metadata (Task ID, Agent, Status, Timestamp)
  - Requirement Summary (3-5 bullet points)
  - Approach & Rationale (kenapa pendekatan ini dipilih)
  - Pattern Reference (pattern yang akan dipakai)
  - Files to Change (new & modified files)
  - Implementation Steps (detail step-by-step)
  - Risks & Considerations (risiko & mitigasi)
  - Test Plan (unit, integration, manual test)
- Set status → `draft`
- **STOP & notify user:** "Proposal siap untuk review di `context/proposal.md`"

### Step 4: Wait for Approval
- Tunggu user approve/reject/request changes
- **Jangan lanjut coding tanpa approval**
- Jika reject → revisi proposal sesuai feedback
- Jika reject 3x → STOP & tanya user: "Ada masalah apa? Mau saya wawancara dulu untuk paham requirement lebih dalam?"

### Step 5: Execute Implementation
- Baca proposal yang sudah approved (jika ada)
- Load pattern yang sudah disebut di proposal
- Implementasi **persis sesuai proposal**:
  - Follow implementation steps
  - Follow pattern yang sudah di-load
  - Tulis kode sesuai files to change
- Jika tidak pakai proposal, ikuti pattern yang relevan

### Step 6: Verify & Document
- Jalankan test (unit, integration, manual)
- Update `delta-handoff.md` dengan perubahan aktual
- Update `code-log.md` dengan log implementasi
- **Jika ada deviasi dari proposal**, dokumentasikan alasan di `code-log.md`:
  - Apa yang berubah dari proposal
  - Kenapa harus deviasi
  - Apakah sudah notify user

### Step 7: Suggest Pattern Update
- Review task yang baru selesai
- Identify learning baru (pola coding baru, best practice baru)
- Suggest update pattern ke user: "Saya detect ada pola baru di task ini. Mau saya tambahkan ke pattern?"
- User approve/reject
- Jika approve, update pattern file yang relevan

---

## 🔍 Workflow Detail: @lcs-review dengan Review Proposal

### Review Checklist untuk Proposal
Jika ada proposal di `context/proposal.md`, review dengan checklist ini:

1. **Completeness**: Apakah semua section terisi? (tidak ada placeholder kosong)
2. **Feasibility**: Apakah approach masuk akal? Apakah bisa diimplementasi?
3. **Pattern Compliance**: Apakah sesuai dengan pattern di `patterns/`? Apakah deviasi jelas alasannya?
4. **Scope**: Apakah scope jelas & tidak terlalu besar? Apakah bisa di-break down lagi?
5. **Risks**: Apakah risiko sudah diidentifikasi? Apakah mitigasi jelas?
6. **Test Plan**: Apakah test plan cukup? Apakah cover happy path & edge case?

### Output Review
- **Jika OK**: Update status proposal → `approved`, notify `@lcs-coding` untuk lanjut implementasi
- **Jika ada masalah**: Update status → `rejected`, berikan feedback spesifik di section "Review Notes", notify user untuk revisi

### Review Checklist untuk Code Implementation
(Existing review checklist tetap berlaku)
