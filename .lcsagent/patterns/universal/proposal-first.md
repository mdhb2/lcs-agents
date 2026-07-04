---
# === OKF METADATA ===
type: "LCS-PATTERN"
title: "Proposal First Execution"
description: "Pattern kapan agent harus buat proposal sebelum coding"
resource: ".lcsagent/patterns/universal/proposal-first.md"
tags: [pattern, proposal, workflow, universal]
timestamp: 2026-07-04T15:50:00Z
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "pattern-index"
lcs-children: []
lcs-session: "pattern-control"
---

# Pattern: Proposal First Execution

## Kapan Dipakai
Agent `@lcs-coding` WAJIB membuat proposal sebelum eksekusi coding jika memenuhi kriteria berikut:

**WAJIB Proposal Jika:**
- Task besar (multi-file, lebih dari 3 file yang terlibat)
- Task complex (banyak edge case, risiko tinggi, arsitektur baru)
- Perubahan kritikal (database schema, security-related, breaking changes)
- Fitur baru (bukan bug fix atau minor change)
- User explicitly bilang: "Pakai proposal" atau "Buat proposal dulu"

**TIDAK PERLU Proposal Jika:**
- Task kecil (1-2 file, bug fix, minor change)
- Task simple (straightforward, low risk, jelas cara implementasinya)
- Update dokumentasi (tidak ada perubahan kode)
- Tweak UI minor (perubahan styling sederhana)
- User explicitly bilang: "Langsung kerjakan" atau "Skip proposal"

**Override:**
User selalu bisa override kriteria default dengan perintah eksplisit.

## Struktur/Template
Gunakan template dari `.lcsagent/templates/proposal.md`

Struktur proposal WAJIB mencakup:
1. Metadata (Task ID, Agent, Status, Timestamp)
2. Requirement Summary (3-5 bullet points)
3. Approach & Rationale (kenapa pendekatan ini dipilih)
4. Pattern Reference (pattern yang akan dipakai)
5. Files to Change (new & modified files)
6. Implementation Steps (detail step-by-step)
7. Risks & Considerations (risiko & mitigasi)
8. Test Plan (unit, integration, manual test)
9. Approval Section (user approve/reject)

## Contoh Implementasi

### Contoh 1: Task Besar (Fitur Baru)
**Task:** "Buat API endpoint untuk user registration dengan email verification"

**Keputusan:** WAJIB proposal

**Alasan:**
- Multi-file (controller, service, model, migration, test)
- Security-related (email verification, password hashing)
- Fitur baru (bukan bug fix)

**Workflow:**
1. Agent baca task di `active-task.md`
2. Agent analisis: "Task ini multi-file dan security-related → butuh proposal"
3. Agent load pattern yang relevan: `api-handler.md`, `error-handling.md`
4. Agent buat proposal di `context/proposal.md`
5. Agent set status → `draft`
6. Agent STOP & notify user: "Proposal siap untuk review di `context/proposal.md`"
7. User review → approve
8. Agent lanjut implementasi sesuai proposal

### Contoh 2: Task Kecil (Bug Fix)
**Task:** "Fix typo di error message: 'Usernme' → 'Username'"

**Keputusan:** TIDAK PERLU proposal

**Alasan:**
- Task kecil (1 file)
- Simple (straightforward fix)
- Low risk (tidak ada impact ke logic)

**Workflow:**
1. Agent baca task di `active-task.md`
2. Agent analisis: "Task ini kecil dan simple → tidak perlu proposal"
3. Agent langsung implementasi (fix typo)
4. Agent update `code-log.md` dengan perubahan

### Contoh 3: Override User
**Task:** "Refactor error handling di semua controller" (task besar)

**User bilang:** "Langsung kerjakan, skip proposal"

**Keputusan:** TIDAK PERLU proposal (override by user)

**Workflow:**
1. Agent baca task di `active-task.md`
2. Agent detect: "Task ini besar → seharusnya butuh proposal"
3. Agent detect override: "User bilang 'skip proposal'"
4. Agent catat di `code-log.md`: "Task ini sebenarnya butuh proposal, tapi user minta langsung kerjakan"
5. Agent langsung implementasi tanpa proposal

## Anti-Pattern (JANGAN Lakukan)
- ❌ **Langsung coding tanpa proposal untuk task besar** - Risiko salah arah, boros token
- ❌ **Buat proposal untuk task kecil yang simple** - Boros waktu, overhead tidak perlu
- ❌ **Proposal tidak lengkap (missing section)** - User tidak bisa review dengan baik
- ❌ **Lanjut coding sebelum user approve proposal** - Melanggar workflow
- ❌ **Deviasi dari proposal tanpa dokumentasi** - Hilang traceability

## Checklist
Agent WAJIB cek item ini sebelum finalize workflow:

### Saat Analisis Task
- [ ] Task sudah dibaca dari `active-task.md`
- [ ] Kriteria proposal sudah di-evaluate (besar/kecil, complex/simple, kritikal/tidak)
- [ ] Override user sudah di-cek (ada perintah eksplisit atau tidak)
- [ ] Keputusan sudah dibuat: butuh proposal atau tidak

### Saat Buat Proposal (Jika Butuh)
- [ ] Template proposal sudah di-copy dari `templates/proposal.md`
- [ ] Semua section sudah terisi (tidak ada placeholder kosong)
- [ ] Pattern reference sudah disebutkan (pattern yang akan dipakai)
- [ ] Files to change sudah detail (new & modified files)
- [ ] Implementation steps sudah jelas (bisa diikuti step-by-step)
- [ ] Risks sudah diidentifikasi (dengan mitigasi)
- [ ] Test plan sudah lengkap (unit, integration, manual)
- [ ] Status proposal set ke `draft`
- [ ] User sudah di-notify untuk review

### Saat Implementasi (Setelah Approval)
- [ ] Proposal sudah approved oleh user
- [ ] Pattern yang disebut di proposal sudah di-load
- [ ] Implementasi sesuai dengan implementation steps di proposal
- [ ] Perubahan file sesuai dengan files to change di proposal
- [ ] Test plan sudah dieksekusi
- [ ] Deviasi dari proposal (jika ada) sudah didokumentasikan di `code-log.md`

### Saat Task Selesai
- [ ] `delta-handoff.md` sudah diupdate dengan perubahan aktual
- [ ] `code-log.md` sudah diupdate dengan log implementasi
- [ ] Test sudah passed
- [ ] Proposal bisa di-archive (pindah status ke `approved` & finalized)

## Catatan
- Proposal adalah **contract** antara agent dan user
- User bisa reject proposal berkali-kali (agent harus revisi sampai approved)
- Jika proposal direject 3x → agent STOP & tanya user: "Ada masalah apa? Mau saya wawancara dulu untuk paham requirement lebih dalam?"
- Proposal berfungsi sebagai dokumentasi keputusan teknis (traceability)
- Proposal menghemat token (mencegah trial-error)
