---
# === OKF METADATA ===
type: "LCS-ACTIVE-TASK"
title: "Active Task - Task Aktif"
description: "Track task yang sedang dikerjakan"
resource: ".lcsagent/templates/active-task.md"
tags: [template, context, task, active]
timestamp: [YYYY-MM-DDTHH:MM:SSZ]
# === LCS CONTRACT METADATA ===
lcs-status: "draft"
lcs-parent: "[session-slug]"
lcs-children: []
lcs-session: "[session-slug]"
---

# Active Task - [Session Slug]

## Session Info
- **Session**: [session-slug]
- **Date**: [YYYY-MM-DD]
- **Agent**: [Agent name yang menjalankan]

## Current Task
- **Task ID**: [Task number dari task.md]
- **Description**: [Deskripsi task yang sedang dikerjakan]
- **Status**: [not-started | in-progress | completed | blocked]

## Files Involved
- `[path/to/file1]` - [Deskripsi]
- `[path/to/file2]` - [Deskripsi]

## Dependencies
- [Task sebelumnya yang harus selesai]
- [External dependency jika ada]

## Checklist
- [ ] [Sub-task 1]
- [ ] [Sub-task 2]
- [ ] [Sub-task 3]

## Notes
[Catatan tambahan tentang task ini]
