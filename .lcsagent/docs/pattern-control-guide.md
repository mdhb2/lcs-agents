---
# === OKF METADATA ===
type: "LCS-GUIDE"
title: "Pattern Control Guide"
description: "Panduan lengkap Pattern Control untuk LCS Agent"
resource: ".lcsagent/docs/pattern-control-guide.md"
tags: [guide, pattern-control, documentation]
timestamp: 2026-07-04T16:15:00Z
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "pattern-control"
lcs-children: []
lcs-session: "pattern-control"
---

# Pattern Control Guide

## Overview
Pattern Control adalah mekanisme agar agent bisa belajar dan mengikuti pola coding spesifik dari codebase yang sudah ada, bukan nebak-nebak gaya coding.

**Manfaat:**
- Kode lebih konsisten dengan pola yang sudah ada
- Hemat token (tidak perlu trial-error)
- Agent belajar dari codebase (bukan dari training data yang outdated)
- Kualitas kode lebih baik (mengikuti best practice project)

## Struktur Folder

```
.lcsagent/
├── PATTERNS.md              # Index semua pattern
└── patterns/                # Folder pattern files
    ├── universal/           # Pattern universal (berlaku semua project)
    │   ├── proposal-first.md
    │   └── tdd-workflow.md
    └── project-specific/    # Pattern project-specific
        ├── api-handler.md
        ├── error-handling.md
        └── database-query.md
```

## Cara Pakai

### 1. Extract Pattern dari Codebase (Bootstrap)

**Kapan**: Project baru pertama kali atau user explicitly request

**Command User**:
```
"Scan codebase dan buat pattern"
```

**Workflow Agent**:
1. Scan struktur folder project
2. Baca file-file kode utama (controller, service, model, dll)
3. Identify pola yang berulang (misal: struktur handler, error handling, dll)
4. Buat pattern files di `patterns/project-specific/`
5. Update `PATTERNS.md` dengan pointer ke pattern baru
6. Tampilkan ringkasan pattern ke user untuk review

**Contoh Output**:
```
Pattern yang berhasil di-extract:

1. API Handler Pattern (patterns/project-specific/api-handler.md)
   - Pola: Controller → Service → Repository
   - Trigger: Keyword "API" / "endpoint"

2. Error Handling Pattern (patterns/project-specific/error-handling.md)
   - Pola: Custom error type + centralized error middleware
   - Trigger: Keyword "error" / "exception"

3. Database Query Pattern (patterns/project-specific/database-query.md)
   - Pola: Repository pattern + parameterized query
   - Trigger: Keyword "database" / "query"

Silakan review pattern di folder .lcsagent/patterns/project-specific/
```

### 2. Load Pattern Saat Coding (Automatic)

**Kapan**: Agent mulai task coding (`@lcs-coding`)

**Workflow Agent**:
1. Baca `PATTERNS.md` (index)
2. Pilih pattern yang relevan berdasarkan keyword di task
3. Load pattern spesifik dari `patterns/[nama].md`
4. Ikuti pola di pattern saat menulis kode
5. Jika pattern tidak ada, agent bisa:
   - Tanya user: "Pattern untuk [kasus ini] belum ada. Mau saya buat?"
   - Atau lanjut coding dengan best practice umum (tapi catat di `code-log.md`)

**Contoh**:
```
Task: "Buat API endpoint GET /users/:id"

Agent detect keyword: "API" / "endpoint"
Agent load pattern: patterns/project-specific/api-handler.md
Agent ikuti pola:
  1. Buat controller di src/controllers/users.js
  2. Buat service di src/services/userService.js
  3. Buat repository di src/repositories/userRepository.js
  4. Ikuti struktur error handling dari pattern
```

### 3. Update Pattern (Maintenance)

**Kapan**: Setelah task selesai, agent suggest update jika ada learning baru

**Workflow Agent**:
1. Review task yang baru selesai
2. Identify learning baru (pola coding baru, best practice baru)
3. Suggest update pattern ke user: "Saya detect ada pola baru di task ini. Mau saya tambahkan ke pattern?"
4. User approve/reject
5. Jika approve, agent update pattern file yang relevan

**Contoh**:
```
Agent: "Saya detect ada pola baru di task ini:
- Anda menggunakan pagination di semua endpoint list
- Anda menggunakan query builder untuk filter dinamis

Mau saya tambahkan pola ini ke pattern api-handler.md?"

User: "Yes"

Agent: "Pattern api-handler.md sudah diupdate dengan:
- Section baru: Pagination Pattern
- Section baru: Dynamic Filter Pattern"
```

### 4. Buat Pattern Baru (On-Demand)

**Kapan**: User request atau agent detect pola berulang (3x)

**Command User**:
```
"Buat pattern baru untuk [nama pattern]"
```

**Workflow Agent**:
1. Identify pola yang berulang
2. Buat pattern baru di `patterns/project-specific/[nama].md`
3. Update `PATTERNS.md` dengan pointer ke pattern baru
4. Notify user: "Pattern baru sudah dibuat: [nama]"

**Contoh**:
```
User: "Buat pattern baru untuk auth middleware"

Agent:
1. Buat patterns/project-specific/auth-middleware.md
2. Update PATTERNS.md dengan pointer
3. Notify: "Pattern auth-middleware.md sudah dibuat. Silakan review & approve."
```

## Trigger Pattern (Automatic)

Agent otomatis load pattern berdasarkan keyword di task:

| Keyword di Task | Pattern yang Di-load |
|:---|:---|
| "API" / "endpoint" / "handler" | `patterns/project-specific/api-handler.md` |
| "error" / "exception" / "handling" | `patterns/project-specific/error-handling.md` |
| "database" / "query" / "repository" | `patterns/project-specific/database-query.md` |
| "test" / "TDD" / "testing" | `patterns/universal/tdd-workflow.md` |
| Task besar / complex / multi-file | `patterns/universal/proposal-first.md` |

## Format Pattern File

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

## Edge Cases

### Pattern Konflik
**Skenario**: Ada 2 pattern yang saling bertentangan

**Solusi**: Agent tanya user "Ada konflik antara pattern A dan B. Mana yang harus saya pakai?"

### Pattern Tidak Ada
**Skenario**: Task butuh pattern tertentu, tapi pattern belum ada

**Solusi**: Agent tanya user "Pattern untuk [kasus ini] belum ada. Mau saya buat?" atau lanjut dengan best practice umum (catat di `code-log.md`)

### Pattern Outdated
**Skenario**: Pattern sudah ada, tapi tech stack berubah atau ada best practice baru

**Solusi**: Agent suggest update pattern setelah task selesai

### Pattern Terlalu Banyak
**Skenario**: Ada terlalu banyak pattern files → agent bingung pilih yang mana

**Solusi**: Agent hanya load pattern yang relevan berdasarkan keyword di task (on-demand loading)

## Best Practices

### Do's ✅
- Buat pattern setelah ada 2-3 implementasi yang mirip (jangan terlalu dini)
- Buat pattern yang spesifik (bukan terlalu general)
- Update pattern secara berkala (jangan biarkan outdated)
- Buat pattern yang testable (ada contoh concrete)

### Don'ts ❌
- Jangan buat pattern untuk kasus one-off (tidak berulang)
- Jangan buat pattern yang terlalu rigid (tidak ada ruang untuk deviasi)
- Jangan load semua pattern sekaligus (boros token)
- Jangan deviasi dari pattern tanpa alasan jelas

## FAQ

### Q: Kapan harus buat pattern baru?
**A**: Buat pattern baru jika:
- Ada pola yang berulang 2-3x di codebase
- User explicitly request
- Ada best practice baru yang perlu didokumentasikan

### Q: Apakah pattern bisa di-override?
**A**: Ya, agent boleh deviasi dari pattern jika ada alasan jelas (dokumentasikan di `code-log.md`)

### Q: Apakah pattern universal dan project-specific beda?
**A**:
- **Universal**: Berlaku di semua project (misal: proposal-first, tdd-workflow)
- **Project-Specific**: Spesifik untuk tech stack project ini (misal: api-handler, error-handling)

### Q: Bagaimana cara hapus pattern yang tidak terpakai?
**A**:
1. Hapus pattern file dari `patterns/`
2. Update `PATTERNS.md` (hapus pointer)
3. Commit perubahan

### Q: Apakah pattern wajib diikuti?
**A**: Ya, kecuali ada alasan jelas untuk deviasi (dokumentasikan di `code-log.md`). Pattern yang dilanggar tanpa alasan = violation di `review.md`.

---

**Dibuat oleh**: LCS Agent  
**Tanggal**: 4 Juli 2026  
**Versi**: 1.0
