---
# === OKF METADATA ===
type: "LCS-PATTERN-INDEX"
title: "Pattern Library - LCS Agent"
description: "Index semua pattern coding yang wajib diikuti agent"
resource: ".lcsagent/PATTERNS.md"
tags: [pattern, index, library]
timestamp: 2026-07-04T15:45:00Z
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "architecture"
lcs-children: ["proposal-first", "tdd-workflow"]
lcs-session: "pattern-control"
---

# Pattern Library - LCS Agent

## Cara Pakai
Agent WAJIB membaca pattern yang relevan SEBELUM menulis kode.

**Aturan:**
1. Jangan load semua pattern sekaligus (boros token)
2. Pilih pattern berdasarkan konteks task (on-demand loading)
3. Ikuti struktur & template di pattern secara ketat
4. Jika pattern tidak ada, buat pattern baru atau lanjut dengan best practice umum (catat di `code-log.md`)
5. Jangan deviasi dari pattern tanpa alasan jelas (dokumentasikan di `code-log.md` jika harus deviasi)

## Daftar Pattern

### Universal
Pattern yang berlaku di semua project (tidak tergantung tech stack):

- `patterns/universal/proposal-first.md` - Kapan harus buat proposal sebelum coding
- `patterns/universal/tdd-workflow.md` - Workflow TDD yang wajib diikuti

### Project-Specific
Pattern yang spesifik untuk tech stack project ini:

*(Belum ada pattern project-specific. Agent bisa buat pattern baru berdasarkan codebase yang sudah ada.)*

## Trigger Pattern
Agent otomatis load pattern berdasarkan keyword di task:

| Keyword di Task | Pattern yang Di-load |
|:---|:---|
| "API" / "endpoint" / "handler" | `patterns/project-specific/api-handler.md` |
| "error" / "exception" / "handling" | `patterns/project-specific/error-handling.md` |
| "database" / "query" / "repository" | `patterns/project-specific/database-query.md` |
| "test" / "TDD" / "testing" | `patterns/universal/tdd-workflow.md` |
| Task besar / complex / multi-file | `patterns/universal/proposal-first.md` |

## Maintenance Pattern

### Extract Pattern dari Codebase
Trigger: User explicitly request "Scan codebase dan buat pattern"

Workflow:
1. Agent scan struktur folder project
2. Agent baca file-file kode utama (controller, service, model, dll)
3. Agent identify pola yang berulang (misal: struktur handler, error handling, dll)
4. Agent buat pattern files di `patterns/project-specific/`
5. Agent update index ini dengan pointer ke pattern baru
6. Agent tampilkan ringkasan pattern ke user untuk review

### Update Pattern
Trigger: Setelah task selesai, agent suggest update jika ada learning baru

Workflow:
1. Agent review task yang baru selesai
2. Agent identify learning baru (pola coding baru, best practice baru)
3. Agent suggest update pattern ke user: "Saya detect ada pola baru di task ini. Mau saya tambahkan ke pattern?"
4. User approve/reject
5. Jika approve, agent update pattern file yang relevan

### Buat Pattern Baru
Trigger: User request atau agent detect pola berulang (3x)

Workflow:
1. Agent identify pola yang berulang
2. Agent buat pattern baru di `patterns/project-specific/[nama].md`
3. Agent update index ini dengan pointer ke pattern baru
4. Agent notify user: "Pattern baru sudah dibuat: [nama]"

## Catatan
- Pattern di-load on-demand (tidak semua pattern di-load sekaligus)
- Pattern yang dilanggar tanpa alasan = violation di `review.md`
- User bisa edit pattern files secara manual kapan saja
