---
# === OKF METADATA ===
type: "LCS-GUIDE"
title: "Proposal First Execution Guide"
description: "Panduan lengkap Proposal First Execution untuk LCS Agent"
resource: ".lcsagent/docs/proposal-first-guide.md"
tags: [guide, proposal-first, documentation]
timestamp: 2026-07-04T16:20:00Z
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "pattern-control"
lcs-children: []
lcs-session: "pattern-control"
---

# Proposal First Execution Guide

## Overview
Proposal First Execution adalah mekanisme agar agent membuat proposal implementasi dulu sebelum coding.

**Manfaat:**
- Mencegah AI ngegas nulis kode yang salah arah
- User bisa review rencana dulu sebelum AI buang-buang token
- Dokumentasi keputusan teknis (traceability)
- Mengurangi trial-error (hemat waktu & token)

## Kriteria Conditional

### WAJIB Proposal Jika:
- Task besar (multi-file, lebih dari 3 file yang terlibat)
- Task complex (banyak edge case, risiko tinggi, arsitektur baru)
- Perubahan kritikal (database schema, security-related, breaking changes)
- Fitur baru (bukan bug fix atau minor change)
- User explicitly bilang: "Pakai proposal" atau "Buat proposal dulu"

### TIDAK PERLU Proposal Jika:
- Task kecil (1-2 file, bug fix, minor change)
- Task simple (straightforward, low risk, jelas cara implementasinya)
- Update dokumentasi (tidak ada perubahan kode)
- Tweak UI minor (perubahan styling sederhana)
- User explicitly bilang: "Langsung kerjakan" atau "Skip proposal"

### Override
User selalu bisa override kriteria default dengan perintah eksplisit.

## Workflow

### Step 1: Analisis Task
Agent baca `active-task.md` dan tentukan apakah task ini butuh proposal atau tidak.

**Contoh**:
```
Task: "Buat API endpoint untuk user registration dengan email verification"

Agent analisis:
- Multi-file: ✅ (controller, service, model, migration, test)
- Security-related: ✅ (email verification, password hashing)
- Fitur baru: ✅ (bukan bug fix)

Keputusan: WAJIB proposal
```

### Step 2: Buat Proposal
Agent load pattern yang relevan dari `patterns/`, buat proposal di `context/proposal.md`, set status → `draft`, **STOP & notify user**.

**Contoh**:
```
Agent: "Proposal siap untuk review di context/proposal.md

Summary:
- Task: Buat API endpoint user registration dengan email verification
- Approach: REST API dengan JWT + email verification token
- Pattern: api-handler.md, error-handling.md
- Files: 5 new files, 2 modified files
- Risks: Email delivery failure, token expiry edge case

Silakan review & approve sebelum saya lanjut implementasi."
```

### Step 3: User Review
User baca proposal dan bisa:
- **Approve**: Update status → `approved`, agent lanjut ke Step 4
- **Reject**: Update status → `rejected`, berikan feedback, agent revisi proposal
- **Request Changes**: Berikan komentar, agent revisi proposal

**Contoh Approve**:
```markdown
## Approval
- [x] User approved
- [ ] User rejected
```

**Contoh Reject**:
```markdown
## Approval
- [ ] User approved
- [x] User rejected (reason: "Approach terlalu complex, pakai library email existing saja")

## Review Notes
Jangan buat email service dari scratch. Pakai library nodemailer yang sudah ada.
```

### Step 4: Execute Implementation
Agent baca proposal yang sudah approved, implementasi **persis sesuai proposal**, load pattern yang sudah disebut, tulis kode sesuai implementation steps.

**Contoh**:
```
Agent implementasi sesuai proposal:

1. Buat controller: src/controllers/authController.js
2. Buat service: src/services/authService.js
3. Buat model: src/models/User.js
4. Buat migration: migrations/20260704_create_users.sql
5. Buat test: test/auth.test.js
6. Update routes: src/routes/index.js
7. Update middleware: src/middleware/auth.js

Pattern dipakai:
- api-handler.md (Controller → Service → Repository)
- error-handling.md (Custom error type)
- tdd-workflow.md (Red-Green-Refactor)
```

### Step 5: Verify & Document
Agent jalankan test, update `delta-handoff.md`, update `code-log.md`. Jika ada deviasi dari proposal, dokumentasikan alasan di `code-log.md`.

**Contoh**:
```
Test Results:
✅ Unit test: PASS (10/10)
✅ Integration test: PASS (5/5)
✅ Manual test: PASS

Deviasi dari proposal:
⚠️ Email verification token menggunakan UUID v4 bukan JWT
Alasan: UUID lebih simple & tidak perlu secret key
User notified: Ya (di code-log.md)
```

## Format Proposal

Gunakan template dari `templates/proposal.md` dengan section:

1. **Metadata** (Task ID, Agent, Status, Timestamp)
2. **Requirement Summary** (3-5 bullet points)
3. **Approach** & Rationale (kenapa pendekatan ini dipilih)
4. **Pattern Reference** (pattern yang akan dipakai)
5. **Files to Change** (new & modified files)
6. **Implementation Steps** (detail step-by-step)
7. **Risks & Considerations** (risiko & mitigasi)
8. **Test Plan** (unit, integration, manual test)
9. **Approval Section** (user approve/reject)

**Contoh**:
```markdown
---
type: "LCS-PROPOSAL"
title: "User Registration dengan Email Verification"
lcs-status: "draft"
---

# Proposal: User Registration dengan Email Verification

## Metadata
- **Task ID**: TASK-001
- **Agent**: @lcs-coding
- **Status**: draft
- **Created**: 2026-07-04T16:00:00Z

## Requirement Summary
- User bisa register dengan email & password
- Sistem kirim email verification setelah register
- User harus verify email sebelum bisa login
- Token verification expire dalam 24 jam

## Approach
REST API dengan JWT authentication + email verification token (UUID v4).

### Kenapa Pendekatan Ini?
- JWT standard untuk authentication
- UUID v4 untuk token verification (simple, tidak perlu secret key)
- Nodemailer untuk email (library proven)

## Pattern Reference
- `patterns/project-specific/api-handler.md` - Struktur controller/service/repository
- `patterns/project-specific/error-handling.md` - Custom error type
- `patterns/universal/tdd-workflow.md` - Red-Green-Refactor

## Files to Change

### New Files
- `src/controllers/authController.js` - Handle register endpoint
- `src/services/authService.js` - Business logic registration
- `src/models/User.js` - User model dengan email verification flag
- `migrations/20260704_create_users.sql` - Database schema
- `test/auth.test.js` - Test registration flow

### Modified Files
- `src/routes/index.js` - Add /auth/register endpoint
- `src/middleware/auth.js` - Add email verification check

## Implementation Steps
1. Buat User model dengan field: email, password, isVerified, verificationToken
2. Buat migration untuk tabel users
3. Buat authService.register() dengan bcrypt password hashing
4. Buat emailService.sendVerificationEmail()
5. Buat authController.register() endpoint POST /auth/register
6. Buat authController.verify() endpoint GET /auth/verify/:token
7. Update authMiddleware untuk cek isVerified
8. Tulis test (unit + integration)

## Risks & Considerations
- ⚠️ Email delivery failure → Mitigasi: Queue + retry mechanism
- ⚠️ Token expiry edge case → Mitigasi: Buat endpoint resend verification
- ⚠️ Password hashing performance → Mitigasi: Bcrypt rounds = 10 (balance security & speed)

## Test Plan
- [ ] Unit test: authService.register()
- [ ] Unit test: emailService.sendVerificationEmail()
- [ ] Integration test: POST /auth/register → return 201
- [ ] Integration test: GET /auth/verify/:token → return 200
- [ ] Integration test: Login sebelum verify → return 403
- [ ] Manual test: Register user baru → cek email inbox

## Approval
- [ ] User approved
- [ ] User rejected (reason: ___)
```

## Edge Cases

### User Tidak Respond
**Skenario**: Agent submit proposal, tapi user tidak respond (lama)

**Solusi**: Agent tunggu sampai user respond. **Jangan lanjut coding tanpa approval.**

### Proposal Direject 3x
**Skenario**: User reject proposal 3x berturut-turut

**Solusi**: Agent STOP & tanya user: "Proposal saya ditolak 3x. Ada masalah apa? Mau saya wawancara dulu untuk paham requirement lebih dalam?"

### Deviasi dari Proposal Saat Implementasi
**Skenario**: Agent mulai implementasi, tapi detect ada masalah → harus deviasi dari proposal

**Solusi**: Agent dokumentasikan deviasi di `code-log.md` dengan alasan jelas. Notify user: "Saya deviasi dari proposal karena [alasan]. Apakah OK?"

### Task Kecil Tapi User Minta Proposal
**Skenario**: Task sebenarnya kecil, tapi user explicitly bilang "pakai proposal"

**Solusi**: Agent tetap buat proposal (override kriteria default)

### Task Besar Tapi User Bilang "Langsung Kerjakan"
**Skenario**: Task sebenarnya besar, tapi user explicitly bilang "langsung kerjakan"

**Solusi**: Agent langsung kerjakan tanpa proposal (override kriteria default). Tapi agent catat di `code-log.md`: "Task ini sebenarnya butuh proposal, tapi user minta langsung kerjakan."

## Best Practices

### Do's ✅
- Buat proposal yang detail (bukan vague)
- Sertakan rationale (kenapa pendekatan ini dipilih vs alternatif lain)
- Sertakan risks & mitigasi (tidak hanya happy path)
- Sertakan test plan yang lengkap (unit, integration, manual)
- Update proposal jika ada feedback dari user

### Don'ts ❌
- Jangan lanjut coding tanpa approval (melanggar workflow)
- Jangan buat proposal yang terlalu panjang (fokus ke poin penting)
- Jangan buat proposal untuk task kecil yang simple (boros waktu)
- Jangan deviasi dari proposal tanpa dokumentasi (hilang traceability)

## FAQ

### Q: Apakah setiap task harus pakai proposal?
**A**: Tidak. Hanya task yang memenuhi kriteria (besar, complex, kritikal, fitur baru). Task kecil/simple tidak perlu proposal.

### Q: Bagaimana jika proposal direject berkali-kali?
**A**: Setelah 3x reject, agent STOP & tanya user untuk wawancara lebih dalam agar paham requirement.

### Q: Apakah boleh deviasi dari proposal?
**A**: Boleh, tapi harus dokumentasikan alasan di `code-log.md` dan notify user.

### Q: Apakah user bisa skip proposal untuk task besar?
**A**: Ya, user bisa override kriteria default dengan perintah eksplisit "Langsung kerjakan" atau "Skip proposal".

### Q: Bagaimana cara approve proposal?
**A**: Edit file `context/proposal.md`, ubah checkbox di section "Approval" dari `[ ]` jadi `[x]`, dan ubah status dari `draft` jadi `approved`.

### Q: Apakah proposal perlu di-commit?
**A**: Ya, proposal harus di-commit sebagai dokumentasi keputusan teknis (traceability).

---

**Dibuat oleh**: LCS Agent  
**Tanggal**: 4 Juli 2026  
**Versi**: 1.0
