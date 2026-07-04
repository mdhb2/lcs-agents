---
# === OKF METADATA ===
type: "LCS-CODE-MAP"
title: "Code Map - Peta Kode"
description: "Peta struktur kode project"
resource: ".lcsagent/templates/code-map.md"
tags: [template, context, code, map]
timestamp: [YYYY-MM-DDTHH:MM:SSZ]
# === LCS CONTRACT METADATA ===
lcs-status: "draft"
lcs-parent: "[session-slug]"
lcs-children: []
lcs-session: "[session-slug]"
---

# Code Map - [Project Name]

## Struktur Folder
```
[project-root]/
├── src/
│   ├── [folder1]/     - [Fungsi]
│   ├── [folder2]/     - [Fungsi]
│   └── [folder3]/     - [Fungsi]
├── tests/
│   └── [folder]/      - [Fungsi]
└── [config files]
```

## File Penting
| File | Fungsi | Keterangan |
| :--- | :--- | :--- |
| `[path/to/file1]` | [Fungsi utama] | [Detail tambahan] |
| `[path/to/file2]` | [Fungsi utama] | [Detail tambahan] |

## Entry Points
| Entry Point | File | Deskripsi |
| :--- | :--- | :--- |
| [Main Entry] | `[path]` | [Deskripsi] |
| [API Entry] | `[path]` | [Deskripsi] |

## Dependencies
| Module | Dependency | Version | Keterangan |
| :--- | :--- | :--- | :--- |
| [Module 1] | [Dependency] | [Version] | [Keterangan] |

## Flow Utama
```
[User Request] → [Entry Point] → [Handler] → [Service] → [Repository] → [Database]
```

## Catatan
[Catatan tambahan tentang struktur kode]
