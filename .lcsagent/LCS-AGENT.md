---
# === OKF METADATA ===
type: "LCS-DOCUMENTATION"
title: "LCS Agent - Source of Truth"
description: "Dokumentasi lengkap LCS Agent workflow, pattern control, dan proposal first execution"
resource: ".lcsagent/LCS-AGENT.md"
tags: [documentation, source-of-truth, workflow, pattern, proposal]
timestamp: 2026-07-04T16:00:00Z
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "architecture"
lcs-children: []
lcs-session: "pattern-control"
---

# LCS Agent - Source of Truth

## 1. Overview
LCS Agent adalah agentic workflow untuk software development yang mengutamakan:
- **Zero-Touch Development**: User tidak menulis kode
- **Compact Context**: Hemat token, baca file yang relevan saja
- **Evidence-Based**: Semua keputusan & perubahan terdokumentasi
- **Pattern-Driven**: Agent belajar dari codebase & ikuti pola yang sudah ada

## 2. Core Principles
1. **Baca Dulu**: WAJIB baca `AGENTS.md` dan `ARCHITECTURE.md` sebelum bekerja
2. **No Assumption**: Jangan berasumsi, TANYA jika ragu
3. **Planning First**: Fokus planning/brainstorming sampai PRD disepakati
4. **Compact Context**: Baca `context-pack.md` & `active-task.md`, jangan baca semua file
5. **Follow Template**: WAJIB gunakan template di `templates/`
6. **Chain of Truth**: Sertakan bukti (log, test, diff), no silent failures
7. **Anti-Looping**: Max 3x iterasi coding, max 2x debug, total max 5x, lalu STOP

## 3. Workflow Stages

### 3.1 Exploration (`@lcs-explore`)
- Brainstorming & debat arsitektur
- Output: `artifacts/[session]/explore.md`

### 3.2 PRD (`@lcs-prd`)
- Susun spesifikasi konkret dari hasil exploration
- Output: `artifacts/[session]/prd.md`

### 3.3 Task Breakdown (`@lcs-task`)
- Pecah PRD jadi checklist tugas atomic
- Output: `artifacts/[session]/task.md`

### 3.4 Pattern Control
Agent WAJIB mengikuti pattern coding yang didefinisikan di `PATTERNS.md`.

#### Struktur
- **Index**: `.lcsagent/PATTERNS.md` - Katalog semua pattern
- **Pattern Files**: `.lcsagent/patterns/[universal|project-specific]/[nama].md` - Detail setiap pattern

#### Aturan
1. **SEBELUM coding**, baca `PATTERNS.md` untuk cek pattern yang relevan
2. Load pattern spesifik dari `patterns/[nama].md` (on-demand, jangan load semua sekaligus)
3. Ikuti struktur & template di pattern secara ketat
4. Jika pattern tidak ada, buat pattern baru di `patterns/` atau lanjut dengan best practice umum (catat di `code-log.md`)
5. Jangan deviasi dari pattern tanpa alasan jelas (dokumentasikan di `code-log.md` jika harus deviasi)

#### Trigger Otomatis
Agent otomatis load pattern berdasarkan keyword di task:
- **"API" / "endpoint"** → `patterns/project-specific/api-handler.md`
- **"error" / "exception"** → `patterns/project-specific/error-handling.md`
- **"database" / "query"** → `patterns/project-specific/database-query.md`
- **"test" / "TDD"** → `patterns/universal/tdd-workflow.md`
- **Task besar / complex** → `patterns/universal/proposal-first.md`

#### Maintenance
- **Setelah task selesai** → agent suggest update pattern (jika ada learning baru)
- **User request** → agent update pattern
- **Agent detect pola berulang (3x)** → agent suggest buat pattern baru

#### Extract Pattern dari Codebase
Trigger: User explicitly request "Scan codebase dan buat pattern"

Workflow:
1. Agent scan struktur folder project
2. Agent baca file-file kode utama (controller, service, model, dll)
3. Agent identify pola yang berulang (misal: struktur handler, error handling, dll)
4. Agent buat pattern files di `patterns/project-specific/`
5. Agent update `PATTERNS.md` dengan pointer ke pattern baru
6. Agent tampilkan ringkasan pattern ke user untuk review

### 3.5 Proposal First Execution
Agent `@lcs-coding` WAJIB membuat proposal sebelum eksekusi coding (jika task besar/complex).

#### Kriteria Default
**WAJIB Proposal Jika:**
- Task besar (multi-file, >3 file terlibat)
- Task complex (banyak edge case, risiko tinggi, arsitektur baru)
- Perubahan kritikal (database schema, security-related, breaking changes)
- Fitur baru (bukan bug fix atau minor change)

**TIDAK PERLU Proposal Jika:**
- Task kecil (1-2 file, bug fix, minor change)
- Task simple (straightforward, low risk, jelas cara implementasinya)
- Update dokumentasi (tidak ada perubahan kode)
- Tweak UI minor (perubahan styling sederhana)

**Override:**
- User bisa explicitly decide per task: "Pakai proposal" atau "Langsung kerjakan"
- User bisa set default per project di `AGENTS.md`

#### Workflow
1. **Analisis task** → tentukan butuh proposal atau tidak
2. **Load pattern** → baca `PATTERNS.md` & load pattern relevan
3. **Buat proposal** → simpan di `context/proposal.md`
4. **Submit proposal** → set status → `draft`, **STOP & notify user**
5. **Wait for approval** → user approve/reject/request changes
6. **Execute implementation** → hanya jika proposal approved
7. **Verify & document** → update `delta-handoff.md` & `code-log.md`
8. **Suggest pattern update** → review task & suggest update pattern

#### Format Proposal
Gunakan template dari `templates/proposal.md` dengan section:
- Metadata (Task ID, Agent, Status, Timestamp)
- Requirement Summary (3-5 bullet points)
- Approach & Rationale (kenapa pendekatan ini dipilih vs alternatif lain)
- Pattern Reference (pattern yang akan dipakai)
- Files to Change (new & modified files)
- Implementation Steps (detail step-by-step)
- Risks & Considerations (risiko & mitigasi)
- Test Plan (unit, integration, manual test)
- Approval Section (user approve/reject)

### 3.6 Coding (`@lcs-coding`)
- Eksekusi implementasi sesuai task & proposal (jika ada)
- Follow pattern yang sudah di-load
- Output: Update `artifacts/[session]/code-log.md`

### 3.7 Review (`@lcs-review`)
- Audit plan/code (interaktif)
- Review proposal (jika ada)
- Output: `artifacts/[session]/review.md`

### 3.8 Debug (`@lcs-debug`)
- Investigasi bug (saat coding macet)
- Output: `artifacts/[session]/debug.md`

### 3.9 Documentation (`@lcs-doc`)
- Rangkum sesi, arsip, bersihkan workspace
- Output: `artifacts/docs/[session]/final.md`

## 4. Artifact Structure

```
.lcsagent/
├── AGENTS.md                  # Pintu masuk & routing agen
├── ARCHITECTURE.md            # Tech stack & konvensi
├── LCS-AGENT.md               # Source of truth (file ini)
├── PATTERNS.md                # Index pattern library
├── patterns/                  # Pattern files
│   ├── universal/             # Pattern universal
│   │   ├── proposal-first.md
│   │   └── tdd-workflow.md
│   └── project-specific/      # Pattern project-specific
│       ├── api-handler.md
│       ├── error-handling.md
│       └── database-query.md
├── templates/                 # Template artifact
│   ├── pattern.md
│   ├── proposal.md
│   ├── explore.md
│   ├── prd.md
│   ├── task.md
│   ├── code-log.md
│   ├── review.md
│   ├── debug.md
│   └── doc.md
├── artifacts/                 # Artifact per session
│   ├── [session-slug]/
│   │   ├── explore.md
│   │   ├── prd.md
│   │   ├── task.md
│   │   ├── code-log.md
│   │   ├── review.md
│   │   └── debug.md
│   └── docs/                  # Documentation archive
│       └── [session-slug]/
│           └── final.md
└── context/                   # Context runtime
    ├── context-pack.md        # Compact context untuk agent
    ├── active-task.md         # Task yang sedang dikerjakan
    ├── proposal.md            # Proposal implementasi (jika ada)
    └── delta-handoff.md       # Perubahan per session
```

## 5. OKF + LCS Contract Metadata

Setiap artifact WAJIB menggunakan format OKF + LCS Contract:

```markdown
---
# === OKF METADATA ===
type: "[LCS-TYPE]"
title: "[Judul Artifact]"
description: "[Deskripsi Singkat]"
resource: "[path/to/artifact]"
tags: [tag1, tag2]
timestamp: [YYYY-MM-DDTHH:MM:SSZ]
# === LCS CONTRACT METADATA ===
lcs-status: "draft" | "final" | "blocked"
lcs-parent: "[parent-artifact-name]"
lcs-children: ["child1", "child2"]
lcs-review-level: "Strict" | "Normal" | "Light"
lcs-session: "[session-slug]"
---
```

### LCS Contract Rules
- **Parent-Child**: Tidak boleh buat artifact baru jika `lcs-parent` belum `final`
- **Traceability**: Setiap artifact WAJIB isi `lcs-parent` (kecuali root artifact)
- **Status**: `draft` → work in progress, `final` → ready for use, `blocked` → stuck/need help

## 6. Context Management

### Compact Context Method
Agent WAJIB membaca file minimal untuk hemat token:
1. **Selalu baca**: `AGENTS.md`, `ARCHITECTURE.md`, `PATTERNS.md`
2. **Baca jika ada**: `context/context-pack.md`, `context/active-task.md`
3. **On-demand**: Pattern files (`patterns/[nama].md`) berdasarkan keyword di task
4. **Jangan baca semua**: Source code, artifact lama, file yang tidak relevan

### Context Files
- **`context-pack.md`**: Ringkasan context (PRD summary, active task, pattern reference, proposal status)
- **`active-task.md`**: Task yang sedang dikerjakan saat ini
- **`proposal.md`**: Proposal implementasi (jika task butuh proposal)
- **`delta-handoff.md`**: Log perubahan per session (untuk handoff ke user/agent lain)

## 7. Edge Cases

### Pattern Control
- **Pattern Konflik**: Agent tanya user "Ada konflik antara pattern A dan B. Mana yang harus saya pakai?"
- **Pattern Tidak Ada**: Agent tanya user "Pattern untuk [kasus ini] belum ada. Mau saya buat?" atau lanjut dengan best practice umum (catat di `code-log.md`)
- **Pattern Outdated**: Agent suggest update pattern setelah task selesai
- **Pattern Terlalu Banyak**: Agent hanya load pattern yang relevan berdasarkan keyword (on-demand loading)

### Proposal First Execution
- **User Tidak Respond**: Agent tunggu sampai user respond, jangan lanjut coding tanpa approval
- **Proposal Direject 3x**: Agent STOP & tanya user: "Ada masalah apa? Mau saya wawancara dulu untuk paham requirement lebih dalam?"
- **Deviasi dari Proposal**: Agent dokumentasikan deviasi di `code-log.md` dengan alasan jelas, notify user
- **User Override**: Agent ikuti perintah eksplisit user (override kriteria default)

## 8. Anti-Looping Mechanism
- **`@lcs-coding`**: Max 3x iterasi
- **`@lcs-debug`**: Max 2x iterasi
- **Total**: Max 5x iterasi (coding + debug)
- **Jika masih gagal**: STOP & set `lcs-status: "blocked"`, notify user untuk manual intervention

## 9. Chain of Truth
Semua keputusan & perubahan WAJIB terdokumentasi dengan bukti:
- **Log**: Catat semua command output, error message, test result
- **Test**: Sertakan test result (pass/fail)
- **Diff**: Sertakan git diff atau file comparison
- **Screenshot**: Sertakan screenshot jika perlu (untuk UI/UX)
- **No Silent Failures**: Jangan skip error atau silent failure, selalu dokumentasikan

## 10. Version
- **Versi**: 2.0
- **Tanggal Update**: 4 Juli 2026
- **Fitur Baru**: Pattern Control, Proposal First Execution
