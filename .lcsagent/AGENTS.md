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
