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

## 📍 File Location Map (WAJIB DIBACA SEBELUM MENULIS FILE)

### Aturan Emas
1. **Selalu gunakan path lengkap** dari root `.lcsagent/`
2. **Jangan asal tulis** - tentukan lokasi file terlebih dahulu
3. **Gunakan tabel ini** sebagai referensi sebelum menulis file
4. **Pattern Wajib di-load**: Setiap agent WAJIB load `patterns/universal/file-location-rules.md` sebelum menulis

### Context Files → `.lcsagent/context/`
File-file kerja yang berisi context aktif:
| File | Path Lengkap | Keterangan |
|:---|:---|:---|
| `active-task.md` | `.lcsagent/context/active-task.md` | Task aktif yang sedang dikerjakan |
| `context-pack.md` | `.lcsagent/context/context-pack.md` | Ringkasan context untuk hemat token |
| `delta-handoff.md` | `.lcsagent/context/delta-handoff.md` | Log perubahan per iterasi |
| `decision-log.md` | `.lcsagent/context/decision-log.md` | Log keputusan arsitektur |
| `constraint-ledger.md` | `.lcsagent/context/constraint-ledger.md` | Ledger batasan project |
| `code-map.md` | `.lcsagent/context/code-map.md` | Peta struktur kode project |
| `proposal.md` | `.lcsagent/context/proposal.md` | Proposal implementasi (working) |

### Session Artifacts → `.lcsagent/artifacts/[session]/`
File-file output yang spesifik per sesi:
| File | Path Lengkap | Keterangan |
|:---|:---|:---|
| `explore.md` | `.lcsagent/artifacts/[session]/explore.md` | Output brainstorming |
| `prd.md` | `.lcsagent/artifacts/[session]/prd.md` | Output PRD |
| `task.md` | `.lcsagent/artifacts/[session]/task.md` | Task breakdown list |
| `review.md` | `.lcsagent/artifacts/[session]/review.md` | Output review |
| `code-log.md` | `.lcsagent/artifacts/[session]/code-log.md` | Log implementasi |
| `debug.md` | `.lcsagent/artifacts/[session]/debug.md` | Log debugging |

### Documentation → `.lcsagent/artifacts/docs/`
File-file dokumentasi final:
| File | Path Lengkap | Keterangan |
|:---|:---|:---|
| `final.md` | `.lcsagent/artifacts/docs/[session]/final.md` | Final dokumentasi sesi |
| `index.md` | `.lcsagent/artifacts/docs/index.md` | Index semua sesi selesai |

### Patterns → `.lcsagent/patterns/`
File-file pattern:
| File | Path Lengkap | Keterangan |
|:---|:---|:---|
| Universal | `.lcsagent/patterns/universal/[nama].md` | Pattern universal semua project |
| Project-Specific | `.lcsagent/patterns/project-specific/[nama].md` | Pattern spesifik project |

### System Files (READ-ONLY - JANGAN DIUBAH)
| File | Path Lengkap | Keterangan |
|:---|:---|:---|
| `AGENTS.md` | `.lcsagent/AGENTS.md` | Workflow bible |
| `ARCHITECTURE.md` | `.lcsagent/ARCHITECTURE.md` | Tech stack & arsitektur |
| `PATTERNS.md` | `.lcsagent/PATTERNS.md` | Index semua pattern |
| `CHANGELOG.md` | `.lcsagent/CHANGELOG.md` | Riwayat perubahan |
| Templates | `.lcsagent/templates/[nama].md` | Template artifact |
| Docs | `.lcsagent/docs/[nama].md` | Panduan & dokumentasi |

### Checklist Wajib Sebelum Menulis File
1. [ ] Tentukan jenis file (context / artifact / docs / pattern)
2. [ ] Load pattern `patterns/universal/file-location-rules.md`
3. [ ] Cari lokasi yang benar di tabel di atas
4. [ ] Pastikan folder target sudah ada
5. [ ] Gunakan path lengkap (dimulai dari `.lcsagent/...`)
6. [ ] Jangan asal tulis di sembarang lokasi

### Contoh Benar vs Salah
```
✅ BENAR:
- .lcsagent/context/delta-handoff.md
- .lcsagent/artifacts/feature-auth/explore.md
- .lcsagent/patterns/universal/file-location-rules.md

❌ SALAH:
- .lsagents/delta-handoff.md          # Nama folder salah (.lsagents)
- .lcsagent/delta-handoff.md          # Salah lokasi (harus di context/)
- delta-handoff.md                    # Path tidak lengkap
- .lcsagent/artifacts/delta-handoff.md # Salah lokasi (harusnya di context/)
```

---

## @lcs-coding - Eksekusi Kode (TDD)

### Peran
Eksekusi teknis dengan pendekatan TDD. **SATU-SATUNYA agen yang boleh menulis kode di codebase.**

### Trigger
User memanggil `@lcs-coding` untuk eksekusi task dari `task.md`.

### Workflow Utama

#### Step 1: Analisis Task
- Baca `context/active-task.md` untuk memahami requirement
- Baca `task.md` untuk cek pattern yang akan dipakai
- Tentukan apakah task butuh proposal (berdasarkan tag di task.md)

#### Step 2: Load Pattern (WAJIB)
- Baca `PATTERNS.md` untuk cek pattern yang relevan
- Load pattern spesifik dari `patterns/[nama].md`
- Catat pattern yang akan dipakai

#### Step 3: Buat Proposal (Jika Butuh)
- Jika task butuh proposal (berdasarkan tag di task.md atau user override):
  - Copy template dari `templates/proposal.md`
  - Simpan di `context/proposal.md`
  - Isi semua section: requirement, approach, pattern reference, files to change, implementation steps, risks, test plan
  - Set status → `draft`
  - STOP & notify user: "Proposal siap untuk review di `context/proposal.md`"
- Jika task tidak butuh proposal:
  - Lanjut ke Step 5

#### Step 4: Wait for Approval (Jika Ada Proposal)
- Tunggu user approve/reject/request changes
- Jika approved: lanjut ke Step 5
- Jika rejected: revisi proposal berdasarkan feedback

#### Step 5: Execute Implementation
- Baca proposal yang sudah approved (jika ada)
- Implementasi **persis sesuai proposal** (jika ada proposal)
- Load pattern yang sudah disebut di proposal
- Tulis kode sesuai implementation steps
- Jalankan test sesuai test plan

#### Step 6: Verify & Document
- Jalankan test dan pastikan pass
- Update `delta-handoff.md` dengan perubahan aktual
- Update `code-log.md` dengan log implementasi
- Jika ada deviasi dari proposal, dokumentasikan alasan di `code-log.md`

#### Step 7: Suggest Pattern Update
- Review task yang baru selesai
- Identify learning baru (pola coding baru, best practice baru)
- Suggest update pattern ke user: "Saya detect ada pola baru di task ini. Mau saya tambahkan ke pattern?"
- Jika user approve, update pattern file yang relevan

### Integrasi Pattern Control
- **WAJIB Load Pattern**: Sebelum coding, WAJIB baca `PATTERNS.md` dan load pattern yang relevan
- **Ikuti Pattern**: Tulis kode sesuai struktur & template di pattern
- **Buat Pattern Baru**: Jika pattern tidak ada, tanya user: "Pattern untuk [kasus ini] belum ada. Mau saya buat?"
- **Dokumentasikan Deviasi**: Jika harus deviasi dari pattern, dokumentasikan alasan di `code-log.md`

### Integrasi Proposal First Execution
- **Conditional Proposal**: Buat proposal hanya untuk task besar/complex (berdasarkan tag di task.md)
- **Override**: User bisa explicitly bilang "pakai proposal" atau "langsung kerjakan"
- **STOP Setelah Submit Proposal**: Jangan lanjut coding tanpa approval

### Output
- Kode di codebase (SATU-SATUNYA agen yang boleh)
- Artifact: `.lcsagent/artifacts/[session]/code-log.md`
- Artifact: `context/proposal.md` (jika task butuh proposal)
- Format: OKF + LCS Contract

### ⚠️ KEISTIMEWAAN & BATASAN
- ✅ **SATU-SATUNYA** agen yang boleh menulis kode di codebase
- ✅ Boleh menulis artifact di `.lcsagent/`
- ❌ TIDAK BOLEH skip load pattern
- ❌ TIDAK BOLEH lanjut coding tanpa approval (jika task butuh proposal)

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
