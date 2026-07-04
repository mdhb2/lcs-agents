---
# === OKF METADATA ===
type: "LCS-PATTERN"
title: "File Location Rules - Aturan Lokasi File"
description: "Aturan penulisan lokasi file untuk mencegah salah kamar"
resource: ".lcsagent/patterns/universal/file-location-rules.md"
tags: [pattern, universal, file-location, rules]
timestamp: [2026-07-04T16:00:00Z]
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "pattern-index"
lcs-children: []
lcs-session: "global"
---

# Pattern: File Location Rules

## Kapan Dipakai
Pattern ini WAJIB diikuti oleh SEMUA agent KETIKA menulis atau memodifikasi file di `.lcsagent/`.

Trigger conditions:
- Sebelum menulis file baru
- Sebelum memodifikasi file yang sudah ada
- Saat copy template
- Saat update context file

## Aturan Utama

### Rule 1: Context Files → `.lcsagent/context/`
File-file yang termasuk context (working files):
| File | Path Lengkap |
|:---|:---|
| `active-task.md` | `.lcsagent/context/active-task.md` |
| `context-pack.md` | `.lcsagent/context/context-pack.md` |
| `delta-handoff.md` | `.lcsagent/context/delta-handoff.md` |
| `decision-log.md` | `.lcsagent/context/decision-log.md` |
| `constraint-ledger.md` | `.lcsagent/context/constraint-ledger.md` |
| `code-map.md` | `.lcsagent/context/code-map.md` |
| `proposal.md` | `.lcsagent/context/proposal.md` |

### Rule 2: Session Artifacts → `.lcsagent/artifacts/[session]/`
File-file output per sesi:
| File | Path Lengkap |
|:---|:---|
| `explore.md` | `.lcsagent/artifacts/[session]/explore.md` |
| `prd.md` | `.lcsagent/artifacts/[session]/prd.md` |
| `task.md` | `.lcsagent/artifacts/[session]/task.md` |
| `review.md` | `.lcsagent/artifacts/[session]/review.md` |
| `code-log.md` | `.lcsagent/artifacts/[session]/code-log.md` |
| `debug.md` | `.lcsagent/artifacts/[session]/debug.md` |

### Rule 3: Documentation → `.lcsagent/artifacts/docs/`
File-file dokumentasi final:
| File | Path Lengkap |
|:---|:---|
| `final.md` | `.lcsagent/artifacts/docs/[session]/final.md` |
| `index.md` | `.lcsagent/artifacts/docs/index.md` |

### Rule 4: Patterns → `.lcsagent/patterns/`
File-file pattern:
| File | Path Lengkap |
|:---|:---|
| Universal | `.lcsagent/patterns/universal/[nama].md` |
| Project-Specific | `.lcsagent/patterns/project-specific/[nama].md` |

### Rule 5: System Files (READ-ONLY)
File-file yang TIDAK BOLEH dimodifikasi agent:
| File | Path Lengkap |
|:---|:---|
| `AGENTS.md` | `.lcsagent/AGENTS.md` |
| `ARCHITECTURE.md` | `.lcsagent/ARCHITECTURE.md` |
| `PATTERNS.md` | `.lcsagent/PATTERNS.md` |
| `CHANGELOG.md` | `.lcsagent/CHANGELOG.md` |
| Templates | `.lcsagent/templates/[nama].md` |
| Docs | `.lcsagent/docs/[nama].md` |

## Checklist Sebelum Menulis File

Agent WAJIB cek item ini SEBELUM menulis file:

- [ ] **Tentukan jenis file**: Apakah ini context, artifact, docs, atau pattern?
- [ ] **Cari lokasi yang benar**: Gunakan tabel di atas sebagai referensi
- [ ] **Pastikan folder ada**: Jika tidak ada, buat folder terlebih dahulu
- [ ] **Gunakan path lengkap**: Selalu gunakan path lengkap dari root `.lcsagent/`
- [ ] **Jangan asal tulis**: Jangan menulis file di sembarang lokasi

## Contoh Benar vs Salah

### ✅ BENAR:
```
# Update delta-handoff.md
File: .lcsagent/context/delta-handoff.md

# Buat explore.md untuk session "feature-auth"
File: .lcsagent/artifacts/feature-auth/explore.md

# Buat pattern baru
File: .lcsagent/patterns/universal/error-handling.md
```

### ❌ SALAH:
```
# JANGAN lakukan ini:
File: .lsagents/delta-handoff.md          # Salah folder (.lsagents vs .lcsagent)
File: .lcsagent/delta-handoff.md          # Salah lokasi (harusnya di context/)
File: delta-handoff.md                    # Salah path (tidak ada folder)
File: .lcsagent/artifacts/delta-handoff.md # Salah lokasi (artifacts untuk session output)
```

## Anti-Pattern (JANGAN Lakukan)
- ❌ Jangan menulis file tanpa menentukan lokasi terlebih dahulu
- ❌ Jangan asal tempel nama file tanpa path lengkap
- ❌ Jangan menulis context file di artifacts atau sebaliknya
- ❌ Jangan menulis file di root `.lcsagent/` (kecuali system files)
- ❌ Jangan menulis file dengan nama yang sudah ada tanpa update

## Verification Command
Untuk memastikan file ditulis di lokasi yang benar, agent bisa menggunakan:
```bash
# Cek apakah file ada di lokasi yang benar
ls -la .lcsagent/context/delta-handoff.md

# Cek apakah file tidak salah tempat
find .lcsagent -name "delta-handoff.md"
```

## Catatan
- Pattern ini berlaku untuk SEMUA agent tanpa kecuali
- Pelanggaran akan dicatat di review.md sebagai violation
- User bisa edit pattern ini kapan saja
