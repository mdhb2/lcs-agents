---
# === OKF METADATA ===
type: "LCS-PRD"
title: "Pattern Control & Proposal First Execution"
description: "Spesifikasi fitur Pattern Control dan Proposal First Execution untuk LCS Agent"
resource: ".lcsagent/artifacts/pattern-control/prd.md"
tags: [pattern-control, proposal-first, workflow, prd]
timestamp: 2026-07-04T15:00:00Z
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "explore"
lcs-children: ["task", "srs"]
lcs-review-level: "Strict"
lcs-session: "pattern-control"
---

# Pattern Control & Proposal First Execution

## 1. Tujuan

Menambahkan dua fitur utama ke LCS Agent:

1. **Pattern Control** - Agar agent bisa belajar dan mengikuti pola coding spesifik dari codebase yang sudah ada, bukan nebak-nebak gaya coding.
2. **Proposal First Execution** - Agar agent membuat proposal implementasi dulu sebelum coding, mencegah AI ngegas dan boros token.

Kedua fitur ini dirancang untuk:
- Meningkatkan kualitas kode (konsisten dengan pola yang sudah ada)
- Menghemat token (tidak perlu trial-error)
- Memberikan kontrol lebih ke user (review proposal sebelum eksekusi)
- Tetap lean & stack-agnostic (bisa dipakai untuk project apa saja)

## 2. Scope

### In Scope
- Struktur folder baru untuk pattern files
- Template pattern dan proposal
- Workflow integration dengan agen yang sudah ada (`@lcs-coding`, `@lcs-review`)
- Mekanisme extract pattern dari codebase
- Mekanisme maintenance pattern (update otomatis)
- Kriteria conditional untuk Proposal First Execution

### Out of Scope
- Implementasi pattern spesifik untuk tech stack tertentu (ini tanggung jawab user/agent per project)
- Integrasi dengan tool external (misal: linter, formatter)
- UI/UX untuk manage pattern (semua via markdown)

## 3. Fitur Utama

### 3.1 Pattern Control

#### 3.1.1 Deskripsi
Pattern Control adalah mekanisme agar agent bisa:
1. **Belajar dari codebase** - Scan codebase yang sudah ada → extract pola coding → simpan sebagai pattern
2. **Mengikuti pattern** - Saat coding, agent load pattern yang relevan dan ikuti polanya
3. **Update pattern** - Setelah task selesai, agent suggest update pattern jika ada learning baru

#### 3.1.2 Scope Pattern
- **Universal Pattern** - Pola yang berlaku di semua project (misal: "selalu buat proposal sebelum coding untuk task besar")
- **Project-Specific Pattern** - Pola yang spesifik untuk tech stack project tertentu (misal: "handler API di project Go pakai struktur X")

#### 3.1.3 Cara Kerja

**A. Extract Pattern dari Codebase**

Trigger:
- User explicitly request: "Scan codebase dan buat pattern"
- Project baru pertama kali di-bootstrap

Workflow:
1. Agent scan struktur folder project
2. Agent baca file-file kode utama (controller, service, model, dll)
3. Agent identify pola yang berulang (misal: struktur handler, error handling, dll)
4. Agent buat pattern files di `.lcsagent/patterns/`
5. Agent buat index di `.lcsagent/PATTERNS.md`
6. Agent tampilkan ringkasan pattern yang sudah dibuat ke user
7. User review & approve pattern

**B. Load Pattern Saat Coding**

Trigger:
- Agent mulai task coding (`@lcs-coding`)

Workflow:
1. Agent baca `PATTERNS.md` (index)
2. Agent pilih pattern yang relevan berdasarkan keyword di task
3. Agent load pattern spesifik dari `patterns/[nama].md`
4. Agent ikuti pola di pattern saat menulis kode
5. Jika pattern tidak ada, agent bisa:
   - Tanya user: "Pattern untuk [kasus ini] belum ada. Mau saya buat?"
   - Atau lanjut coding dengan best practice umum (tapi catat di `code-log.md`)

**C. Maintenance Pattern**

Trigger:
- Setelah task selesai (agent suggest update)
- User explicitly request: "Update pattern"
- Agent detect pola baru yang berulang (misal: user minta fitur yang mirip 3x)

Workflow:
1. Agent review task yang baru selesai
2. Agent identify learning baru (misal: pola coding baru, best practice baru)
3. Agent suggest update pattern ke user: "Saya detect ada pola baru di task ini. Mau saya tambahkan ke pattern?"
4. User approve/reject
5. Jika approve, agent update pattern file yang relevan

#### 3.1.4 Struktur Folder

```
.lcsagent/
├── PATTERNS.md              # Index semua pattern
├── patterns/                # Folder pattern files
│   ├── universal/           # Pattern universal (berlaku semua project)
│   │   ├── proposal-first.md
│   │   └── tdd-workflow.md
│   └── project-specific/    # Pattern project-specific
│       ├── api-handler.md
│       ├── error-handling.md
│       └── database-query.md
```

#### 3.1.5 Format Pattern File

Setiap pattern file menggunakan format OKF + LCS Contract:

```markdown
---
# === OKF METADATA ===
type: "LCS-PATTERN"
title: "[Nama Pattern]"
description: "[Deskripsi singkat]"
resource: ".lcsagent/patterns/[nama].md"
tags: [tag1, tag2]
timestamp: [ISO timestamp]
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "pattern-index"
lcs-children: []
lcs-session: "[session name]"
---

# Pattern: [Nama Pattern]

## Kapan Dipakai
[Deskripsi kapan pattern ini harus dipakai]

## Struktur/Template
[Struktur atau template kode yang harus diikuti]

## Contoh Implementasi
[Contoh kode konkret yang mengikuti pattern]

## Anti-Pattern (JANGAN Lakukan)
- ❌ [Hal yang tidak boleh dilakukan]
- ❌ [Hal yang tidak boleh dilakukan]

## Checklist
- [ ] [Item checklist 1]
- [ ] [Item checklist 2]
```

#### 3.1.6 Format PATTERNS.md (Index)

```markdown
---
# === OKF METADATA ===
type: "LCS-PATTERN-INDEX"
title: "Pattern Library - LCS Agent"
description: "Index semua pattern coding yang wajib diikuti agent"
resource: ".lcsagent/PATTERNS.md"
tags: [pattern, index]
timestamp: [ISO timestamp]
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "architecture"
lcs-children: ["api-handler", "error-handling", "database-query"]
lcs-session: "[session name]"
---

# Pattern Library - LCS Agent

## Cara Pakai
Agent WAJIB membaca pattern yang relevan SEBELUM menulis kode.
Jangan load semua pattern sekaligus (boros token).
Pilih pattern berdasarkan konteks task.

## Daftar Pattern

### Universal
- `patterns/universal/proposal-first.md` - Kapan harus buat proposal sebelum coding
- `patterns/universal/tdd-workflow.md` - Workflow TDD yang wajib diikuti

### Project-Specific
- `patterns/project-specific/api-handler.md` - Struktur handler API
- `patterns/project-specific/error-handling.md` - Custom error type & error response
- `patterns/project-specific/database-query.md` - Repository pattern & parameterized query

## Trigger Pattern
Agent otomatis load pattern berdasarkan keyword di task:
- "API" / "endpoint" / "handler" → load `api-handler.md`
- "error" / "exception" → load `error-handling.md`
- "database" / "query" → load `database-query.md`
- "test" / "TDD" → load `tdd-workflow.md`
- Task besar / complex → load `proposal-first.md`
```

### 3.2 Proposal First Execution

#### 3.2.1 Deskripsi
Proposal First Execution adalah mekanisme agar agent membuat proposal implementasi dulu sebelum coding. Tujuannya:
- Mencegah AI ngegas nulis kode yang salah arah
- User bisa review rencana dulu sebelum AI buang-buang token
- Dokumentasi keputusan teknis (traceability)

#### 3.2.2 Kriteria Conditional

**Default: WAJIB Proposal Jika:**
- Task besar (multi-file, arsitektur baru)
- Task complex (banyak edge case, risiko tinggi)
- Perubahan kritikal (database schema, security-related)
- Fitur baru (bukan bug fix)

**Default: TIDAK PERLU Proposal Jika:**
- Task kecil (bug fix, minor change)
- Task simple (straightforward, low risk)
- Update dokumentasi
- Tweak UI minor

**Override:**
- User bisa explicitly bilang: "Pakai proposal" atau "Langsung kerjakan"
- User bisa set default per project di `AGENTS.md`

#### 3.2.3 Workflow

**Step 1: Analisis Task**
- Agent baca `active-task.md`
- Agent tentukan apakah task ini butuh proposal atau tidak (berdasarkan kriteria di atas)

**Step 2: Jika Butuh Proposal**
- Agent load pattern yang relevan dari `patterns/`
- Agent buat proposal di `context/proposal.md` (pakai template)
- Agent set status proposal → `draft`
- Agent STOP & notify user: "Proposal siap untuk review di `context/proposal.md`"

**Step 3: User Review**
- User baca proposal
- User bisa:
  - **Approve**: Update status → `approved`, agent lanjut ke Step 4
  - **Reject**: Update status → `rejected`, berikan feedback, agent revisi proposal
  - **Request Changes**: Berikan komentar, agent revisi proposal

**Step 4: Execute Implementation**
- Agent baca proposal yang sudah approved
- Agent implementasi **persis sesuai proposal**
- Agent load pattern yang sudah disebut di proposal
- Agent tulis kode sesuai implementation steps

**Step 5: Verify & Document**
- Agent jalankan test
- Agent update `delta-handoff.md` dengan perubahan aktual
- Agent update `code-log.md` dengan log implementasi
- Jika ada deviasi dari proposal, agent dokumentasikan alasan di `code-log.md`

#### 3.2.4 Format Proposal Template

```markdown
---
# === OKF METADATA ===
type: "LCS-PROPOSAL"
title: "[Judul Task]"
description: "Proposal implementasi untuk [task]"
resource: ".lcsagent/context/proposal.md"
tags: [proposal, planning]
timestamp: [ISO timestamp]
# === LCS CONTRACT METADATA ===
lcs-status: "draft"
lcs-parent: "active-task"
lcs-children: []
lcs-session: "[session name]"
---

# Proposal: [Judul Task]

## Metadata
- **Task ID**: [ID dari active-task.md]
- **Agent**: @lcs-coding
- **Status**: draft | approved | rejected
- **Created**: [timestamp]
- **Approved By**: [user/timestamp jika approved]

## Requirement Summary
[Rangkuman requirement dari active-task.md dalam 3-5 bullet points]

- Requirement 1
- Requirement 2
- Requirement 3

## Approach
[Penjelasan pendekatan teknis yang dipilih]

### Kenapa Pendekatan Ini?
[Alasan memilih pendekatan ini vs alternatif lain]

## Pattern Reference
- Pattern yang dipakai: `patterns/[nama].md`
- Deviasi dari pattern (jika ada): [jelaskan alasan]

## Files to Change

### New Files
- `src/modules/[module]/[file].js` - [Deskripsi fungsi]
- `src/modules/[module]/[file].js` - [Deskripsi fungsi]

### Modified Files
- `src/routes/index.js` - [Apa yang dimodifikasi]
- `src/database/schema.js` - [Apa yang dimodifikasi]

## Implementation Steps
1. [Step 1]
2. [Step 2]
3. [Step 3]
4. [Step 4]

## Risks & Considerations
- [Risiko 1]
- [Risiko 2]

## Test Plan
- [ ] Unit test untuk [component/service]
- [ ] Integration test untuk [API endpoint]
- [ ] Manual test: [skenario test]

## Approval
- [ ] User approved
- [ ] User rejected (reason: ___)
```

## 4. Spesifikasi Teknis

### 4.1 Integrasi dengan Agen yang Ada

#### 4.1.1 Update `@lcs-coding` di `AGENTS.md`

Tambahkan workflow baru:

```markdown
## @lcs-coding - Workflow dengan Pattern Control & Proposal First

### Step 1: Analisis Task
- Baca `context/active-task.md`
- Tentukan apakah task butuh proposal (lihat kriteria di section 3.2.2)

### Step 2: Load Pattern
- Baca `PATTERNS.md` untuk cek pattern yang relevan
- Load pattern spesifik dari `patterns/[nama].md`
- Catat pattern yang akan dipakai

### Step 3: Buat Proposal (Jika Butuh)
- Copy template dari `templates/proposal.md`
- Simpan di `context/proposal.md`
- Isi semua section
- Set status → `draft`
- STOP & notify user

### Step 4: Wait for Approval
- Tunggu user approve/reject/request changes

### Step 5: Execute Implementation
- Baca proposal yang sudah approved
- Implementasi sesuai proposal
- Load pattern yang sudah disebut
- Tulis kode sesuai implementation steps

### Step 6: Verify & Document
- Jalankan test
- Update `delta-handoff.md`
- Update `code-log.md`
- Dokumentasikan deviasi dari proposal (jika ada)

### Step 7: Suggest Pattern Update
- Review task yang baru selesai
- Identify learning baru
- Suggest update pattern ke user (jika ada)
```

#### 4.1.2 Update `@lcs-review` di `AGENTS.md`

Tambahkan capability review proposal:

```markdown
## @lcs-review - Review Proposal

### Review Checklist untuk Proposal
1. **Completeness**: Apakah semua section terisi?
2. **Feasibility**: Apakah approach masuk akal?
3. **Pattern Compliance**: Apakah sesuai dengan pattern di `patterns/`?
4. **Scope**: Apakah scope jelas & tidak terlalu besar?
5. **Risks**: Apakah risiko sudah diidentifikasi?
6. **Test Plan**: Apakah test plan cukup?

### Output Review
- Jika OK: Update status proposal → `approved`
- Jika ada masalah: Update status → `rejected`, berikan feedback spesifik
```

### 4.2 Update `context-pack.md`

Tambahkan pointer ke proposal & pattern:

```markdown
## Proposal
- File: `context/proposal.md`
- Status: [not-created | draft | approved | rejected]
- Note: Proposal harus di-approve sebelum coding dimulai

## Pattern Reference
- Index: `PATTERNS.md`
- Loaded patterns: [list pattern yang sedang dipakai]
```

### 4.3 Update `LCS-AGENT.md` (Source of Truth)

Tambahkan section baru:

```markdown
### 3.4 Pattern Control

Agent WAJIB mengikuti pattern coding yang didefinisikan di `PATTERNS.md`.

#### Struktur
- **Index**: `.lcsagent/PATTERNS.md` - Katalog semua pattern
- **Pattern Files**: `.lcsagent/patterns/[nama].md` - Detail setiap pattern

#### Aturan
1. SEBELUM coding, baca `PATTERNS.md` untuk cek pattern yang relevan
2. Load pattern spesifik dari `patterns/[nama].md`
3. Ikuti struktur & template di pattern secara ketat
4. Jika pattern tidak ada, buat pattern baru di `patterns/`
5. Jangan deviasi dari pattern tanpa alasan jelas

#### Trigger Otomatis
Agent otomatis load pattern berdasarkan keyword di task:
- "API" / "endpoint" → `patterns/project-specific/api-handler.md`
- "error" / "exception" → `patterns/project-specific/error-handling.md`
- "database" / "query" → `patterns/project-specific/database-query.md`
- "test" / "TDD" → `patterns/universal/tdd-workflow.md`
- Task besar / complex → `patterns/universal/proposal-first.md`

#### Maintenance
- Setelah task selesai → agent suggest update pattern
- User request → agent update pattern
- Agent detect pola berulang → agent suggest buat pattern baru

### 3.5 Proposal First Execution

Agent `@lcs-coding` WAJIB membuat proposal sebelum eksekusi coding (jika task besar/complex).

#### Kriteria Default
- **WAJIB Proposal**: Task besar, complex, perubahan kritikal, fitur baru
- **TIDAK PERLU Proposal**: Task kecil, simple, update dokumentasi, tweak UI minor
- **Override**: User bisa explicitly decide per task

#### Workflow
1. Analisis task → tentukan butuh proposal atau tidak
2. Load pattern → baca `PATTERNS.md` & load pattern relevan
3. Buat proposal → simpan di `context/proposal.md`
4. Submit proposal → set status → `draft`, STOP & notify user
5. Wait for approval → user approve/reject/request changes
6. Execute implementation → hanya jika proposal approved
7. Verify & document → update `delta-handoff.md` & `code-log.md`
8. Suggest pattern update → review task & suggest update pattern
```

## 5. Edge Cases

### 5.1 Pattern Control

**Edge Case 1: Pattern Konflik**
- **Skenario**: Ada 2 pattern yang saling bertentangan (misal: pattern A bilang "pakai X", pattern B bilang "pakai Y")
- **Solusi**: Agent tanya user: "Ada konflik antara pattern A dan B. Mana yang harus saya pakai?"

**Edge Case 2: Pattern Tidak Ada**
- **Skenario**: Task butuh pattern tertentu, tapi pattern belum ada
- **Solusi**: Agent tanya user: "Pattern untuk [kasus ini] belum ada. Mau saya buat?" atau agent lanjut dengan best practice umum (catat di `code-log.md`)

**Edge Case 3: Pattern Outdated**
- **Skenario**: Pattern sudah ada, tapi tech stack berubah atau ada best practice baru
- **Solusi**: Agent suggest update pattern setelah task selesai

**Edge Case 4: Pattern Terlalu Banyak**
- **Skenario**: Ada terlalu banyak pattern files → agent bingung pilih yang mana
- **Solusi**: Agent hanya load pattern yang relevan berdasarkan keyword di task (on-demand loading)

### 5.2 Proposal First Execution

**Edge Case 1: User Tidak Respond**
- **Skenario**: Agent submit proposal, tapi user tidak respond (lama)
- **Solusi**: Agent tunggu sampai user respond. Jangan lanjut coding tanpa approval.

**Edge Case 2: Proposal Direject Berkali-kali**
- **Skenario**: User reject proposal 3x berturut-turut
- **Solusi**: Agent STOP & tanya user: "Proposal saya ditolak 3x. Ada masalah apa? Mau saya wawancara dulu untuk paham requirement lebih dalam?"

**Edge Case 3: Deviasi dari Proposal Saat Implementasi**
- **Skenario**: Agent mulai implementasi, tapi detect ada masalah → harus deviasi dari proposal
- **Solusi**: Agent dokumentasikan deviasi di `code-log.md` dengan alasan jelas. Notify user: "Saya deviasi dari proposal karena [alasan]. Apakah OK?"

**Edge Case 4: Task Kecil Tapi User Minta Proposal**
- **Skenario**: Task sebenarnya kecil, tapi user explicitly bilang "pakai proposal"
- **Solusi**: Agent tetap buat proposal (override kriteria default)

**Edge Case 5: Task Besar Tapi User Bilang "Langsung Kerjakan"**
- **Skenario**: Task sebenarnya besar, tapi user explicitly bilang "langsung kerjakan"
- **Solusi**: Agent langsung kerjakan tanpa proposal (override kriteria default). Tapi agent catat di `code-log.md`: "Task ini sebenarnya butuh proposal, tapi user minta langsung kerjakan."

## 6. Acceptance Criteria

### 6.1 Pattern Control

- [ ] Folder `.lcsagent/patterns/` sudah dibuat
- [ ] File `PATTERNS.md` (index) sudah dibuat dengan format OKF
- [ ] Pattern files bisa dibuat dengan format OKF + LCS Contract
- [ ] Agent bisa extract pattern dari codebase yang sudah ada
- [ ] Agent bisa load pattern yang relevan saat coding
- [ ] Agent bisa suggest update pattern setelah task selesai
- [ ] Agent bisa buat pattern baru on-demand dari user request
- [ ] Pattern yang dilanggar tanpa alasan = violation di `review.md`

### 6.2 Proposal First Execution

- [ ] Template proposal sudah dibuat di `templates/proposal.md`
- [ ] Agent bisa tentukan apakah task butuh proposal atau tidak (berdasarkan kriteria)
- [ ] Agent bisa buat proposal dengan format OKF + LCS Contract
- [ ] Agent STOP setelah submit proposal (tidak lanjut coding tanpa approval)
- [ ] Agent bisa terima approval/rejection/request changes dari user
- [ ] Agent implementasi sesuai proposal yang sudah approved
- [ ] Agent dokumentasikan deviasi dari proposal (jika ada) di `code-log.md`
- [ ] User bisa override kriteria default (explicitly bilang "pakai proposal" atau "langsung kerjakan")

### 6.3 Integrasi

- [ ] `AGENTS.md` sudah diupdate dengan workflow baru untuk `@lcs-coding`
- [ ] `AGENTS.md` sudah diupdate dengan capability review proposal untuk `@lcs-review`
- [ ] `context-pack.md` sudah diupdate dengan pointer ke proposal & pattern
- [ ] `LCS-AGENT.md` (Source of Truth) sudah diupdate dengan section 3.4 & 3.5
- [ ] Workflow end-to-end bisa dijalankan tanpa error

## 7. Dependencies

- **Opencode CLI** - Tool untuk menjalankan agen
- **Zed Editor** - Code editor yang terintegrasi dengan Opencode
- **Markdown** - Format untuk semua artifact & pattern files

## 8. Risks

### 8.1 Pattern Control

**Risk 1: Pattern Salah Extract**
- **Deskripsi**: Agent extract pattern yang salah dari codebase (misal: extract anti-pattern)
- **Mitigasi**: User review & approve pattern sebelum dipakai

**Risk 2: Pattern Terlalu Kaku**
- **Deskripsi**: Agent terlalu ketat ikuti pattern → tidak bisa adaptasi ke kasus khusus
- **Mitigasi**: Agent boleh deviasi dari pattern jika ada alasan jelas (dokumentasikan di `code-log.md`)

### 8.2 Proposal First Execution

**Risk 1: Proposal Overhead**
- **Deskripsi**: Buat proposal untuk task kecil → boros waktu & token
- **Mitigasi**: Kriteria conditional (task kecil tidak perlu proposal)

**Risk 2: Proposal Tidak Akurat**
- **Deskripsi**: Proposal dibuat, tapi saat implementasi ada masalah → harus deviasi
- **Mitigasi**: Agent dokumentasikan deviasi di `code-log.md` dengan alasan jelas

**Risk 3: User Tidak Review Proposal**
- **Deskripsi**: Agent submit proposal, tapi user tidak review → agent stuck menunggu
- **Mitigasi**: Agent tunggu sampai user respond. Jangan lanjut coding tanpa approval.

## 9. Timeline

- **Hari 1**: Setup struktur folder & file (PATTERNS.md, patterns/, templates/proposal.md)
- **Hari 2**: Update AGENTS.md, context-pack.md, LCS-AGENT.md
- **Hari 3**: Test workflow end-to-end
- **Hari 4**: Refine berdasarkan feedback

## 10. Catatan Tambahan

- **Prinsip Utama**: "Semua mudah dipahamin, hemat token, evidence-based, dan bersih."
- **Stack-Agnostic**: Pattern & proposal tidak terikat tech stack tertentu
- **Lean**: Pattern di-load on-demand (tidak semua pattern di-load sekaligus)
- **Traceable**: Semua proposal & pattern terdokumentasi, bisa diaudit nanti

---

**Dibuat oleh**: User & AI Collaboratively  
**Tanggal**: 4 Juli 2026  
**Status**: Final (siap untuk dieksekusi oleh `@lcs-coding`)
