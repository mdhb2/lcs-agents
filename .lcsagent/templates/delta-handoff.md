---
# === OKF METADATA ===
type: "LCS-HANDOFF"
title: "Delta Handoff - Perubahan Per Session"
description: "Log perubahan untuk handoff ke user/agent lain"
resource: ".lcsagent/templates/delta-handoff.md"
tags: [handoff, delta, log, template]
timestamp: [YYYY-MM-DDTHH:MM:SSZ]
# === LCS CONTRACT METADATA ===
lcs-status: "draft"
lcs-parent: "[session-slug]"
lcs-children: []
lcs-session: "[session-slug]"
---

# Delta Handoff - [Session Slug]

## Session Info
- **Session**: [session-slug]
- **Date**: [YYYY-MM-DD]
- **Agent**: [Agent name yang menjalankan]
- **Status**: [in-progress | completed | blocked]

## Changes Overview
[Ringkasan perubahan dalam session ini]

### Files Changed
- **New Files**: [jumlah file baru]
- **Modified Files**: [jumlah file dimodifikasi]
- **Deleted Files**: [jumlah file dihapus]

### Tasks Completed
- [ ] [Task 1]
- [ ] [Task 2]

## Detailed Changes

### [Task ID]: [Task Name]

#### Files Changed
- ✅ **New**: `[path/to/file]` - [Deskripsi fungsi]
- ✏️ **Modified**: `[path/to/file]` - [Apa yang dimodifikasi]
- ❌ **Deleted**: `[path/to/file]` - [Alasan dihapus]

#### Implementation Summary
[Ringkasan implementasi untuk task ini]

#### Pattern Used
- `patterns/[nama].md` - [Pattern yang dipakai]

#### Deviasi dari Proposal (Jika Ada)
- ⚠️ [Apa yang berubah dari proposal]
- **Alasan**: [Kenapa harus deviasi]
- **User Notified**: [Ya/Tidak]

#### Test Results
- ✅ Unit test: [PASS/FAIL]
- ✅ Integration test: [PASS/FAIL]
- ✅ Manual test: [PASS/FAIL]

#### Blockers Encountered
- [Blocker 1 - status: resolved/unresolved]
- [Blocker 2 - status: resolved/unresolved]

---

## Next Session
[Action items untuk session berikutnya]

- [ ] [Action 1]
- [ ] [Action 2]

## Handoff Notes
[Catatan penting untuk user/agent yang akan lanjut session ini]
