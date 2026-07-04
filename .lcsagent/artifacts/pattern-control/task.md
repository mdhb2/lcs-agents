---
# === OKF METADATA ===
type: "LCS-TASK"
title: "Pattern Control & Proposal First Execution - Task Breakdown"
description: "Checklist tugas implementasi Pattern Control dan Proposal First Execution"
resource: ".lcsagent/artifacts/pattern-control/task.md"
tags: [pattern-control, proposal-first, task-breakdown]
timestamp: 2026-07-04T15:30:00Z
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "prd"
lcs-children: ["code-log"]
lcs-review-level: "Strict"
lcs-session: "pattern-control"
---

# Pattern Control & Proposal First Execution - Task Breakdown

## FASE 1: Setup Struktur & Template

### TASK-001: Buat Struktur Folder Pattern
- [ ] Buat folder `.lcsagent/patterns/`
- [ ] Buat subfolder `.lcsagent/patterns/universal/`
- [ ] Buat subfolder `.lcsagent/patterns/project-specific/`
- **File terlibat:** (folder baru)
- **Dependency:** None
- **Kompleksitas:** Simple
- **Estimasi:** 5 menit

### TASK-002: Buat Template Pattern File
- [ ] Buat file `.lcsagent/templates/pattern.md`
- [ ] Isi template dengan struktur OKF + LCS Contract
- [ ] Include section: Kapan Dipakai, Struktur/Template, Contoh Implementasi, Anti-Pattern, Checklist
- **File terlibat:** `.lcsagent/templates/pattern.md`
- **Dependency:** None
- **Kompleksitas:** Simple
- **Estimasi:** 10 menit

### TASK-003: Buat Template Proposal File
- [ ] Buat file `.lcsagent/templates/proposal.md`
- [ ] Isi template dengan struktur OKF + LCS Contract
- [ ] Include section: Metadata, Requirement Summary, Approach, Pattern Reference, Files to Change, Implementation Steps, Risks, Test Plan, Approval
- **File terlibat:** `.lcsagent/templates/proposal.md`
- **Dependency:** None
- **Kompleksitas:** Simple
- **Estimasi:** 15 menit

### TASK-004: Buat File PATTERNS.md (Index)
- [ ] Buat file `.lcsagent/PATTERNS.md`
- [ ] Isi dengan struktur OKF + LCS Contract
- [ ] Include section: Cara Pakai, Daftar Pattern (Universal & Project-Specific), Trigger Pattern
- [ ] Set `lcs-status: "draft"` (akan diubah jadi "final" setelah pattern pertama dibuat)
- **File terlibat:** `.lcsagent/PATTERNS.md`
- **Dependency:** TASK-001, TASK-002
- **Kompleksitas:** Simple
- **Estimasi:** 15 menit

---

## FASE 2: Buat Universal Pattern (Bootstrap)

### TASK-005: Buat Pattern "Proposal First"
- [ ] Buat file `.lcsagent/patterns/universal/proposal-first.md`
- [ ] Definisikan kriteria kapan harus buat proposal
- [ ] Buat workflow step-by-step
- [ ] Tambahkan contoh kasus & anti-pattern
- [ ] Update `PATTERNS.md` dengan pointer ke pattern ini
- **File terlibat:** `.lcsagent/patterns/universal/proposal-first.md`, `.lcsagent/PATTERNS.md`
- **Dependency:** TASK-002, TASK-004
- **Kompleksitas:** Medium
- **Estimasi:** 20 menit

### TASK-006: Buat Pattern "TDD Workflow"
- [ ] Buat file `.lcsagent/patterns/universal/tdd-workflow.md`
- [ ] Definisikan workflow TDD (Red-Green-Refactor)
- [ ] Buat checklist untuk setiap step
- [ ] Tambahkan contoh test case & anti-pattern
- [ ] Update `PATTERNS.md` dengan pointer ke pattern ini
- **File terlibat:** `.lcsagent/patterns/universal/tdd-workflow.md`, `.lcsagent/PATTERNS.md`
- **Dependency:** TASK-002, TASK-004
- **Kompleksitas:** Medium
- **Estimasi:** 20 menit

---

## FASE 3: Update Documentation Core

### TASK-007: Update AGENTS.md - Tambah Workflow @lcs-coding
- [ ] Baca `.lcsagent/AGENTS.md`
- [ ] Tambahkan section workflow baru untuk `@lcs-coding` dengan Pattern Control & Proposal First
- [ ] Include 7 step: Analisis Task, Load Pattern, Buat Proposal, Wait for Approval, Execute Implementation, Verify & Document, Suggest Pattern Update
- [ ] Tambahkan trigger otomatis pattern berdasarkan keyword
- **File terlibat:** `.lcsagent/AGENTS.md`
- **Dependency:** TASK-005, TASK-006
- **Kompleksitas:** Medium
- **Estimasi:** 25 menit

### TASK-008: Update AGENTS.md - Tambah Capability @lcs-review
- [ ] Baca `.lcsagent/AGENTS.md`
- [ ] Tambahkan section review proposal untuk `@lcs-review`
- [ ] Include checklist: Completeness, Feasibility, Pattern Compliance, Scope, Risks, Test Plan
- [ ] Include output review: approve/reject workflow
- **File terlibat:** `.lcsagent/AGENTS.md`
- **Dependency:** TASK-003
- **Kompleksitas:** Simple
- **Estimasi:** 15 menit

### TASK-009: Buat LCS-AGENT.md (Source of Truth)
- [ ] Cek apakah `.lcsagent/LCS-AGENT.md` sudah ada
- [ ] Jika belum ada, buat file baru dengan struktur OKF + LCS Contract
- [ ] Tambahkan section 3.4 "Pattern Control" (struktur, aturan, trigger otomatis, maintenance)
- [ ] Tambahkan section 3.5 "Proposal First Execution" (kriteria default, workflow)
- **File terlibat:** `.lcsagent/LCS-AGENT.md`
- **Dependency:** TASK-007, TASK-008
- **Kompleksitas:** Medium
- **Estimasi:** 30 menit

---

## FASE 4: Update Context System

### TASK-010: Update Template context-pack.md
- [ ] Baca `.lcsagent/templates/context-pack.md` (jika ada) atau buat baru
- [ ] Tambahkan section "Proposal" dengan field: File, Status, Note
- [ ] Tambahkan section "Pattern Reference" dengan field: Index, Loaded patterns
- [ ] Set placeholder untuk status proposal: `[not-created | draft | approved | rejected]`
- **File terlibat:** `.lcsagent/templates/context-pack.md`
- **Dependency:** TASK-003, TASK-004
- **Kompleksitas:** Simple
- **Estimasi:** 10 menit

### TASK-011: Update delta-handoff.md Template (Jika Ada)
- [ ] Cek apakah `.lcsagent/templates/delta-handoff.md` sudah ada
- [ ] Jika ada, tambahkan section untuk log deviasi dari proposal
- [ ] Jika belum ada, skip task ini (bukan blocker)
- **File terlibat:** `.lcsagent/templates/delta-handoff.md`
- **Dependency:** None
- **Kompleksitas:** Simple
- **Estimasi:** 10 menit (optional)

---

## FASE 5: Integrasi & Testing

### TASK-012: Test Workflow Pattern Control - Extract Pattern
- [ ] Simulasi workflow: "Scan codebase dan buat pattern"
- [ ] Verifikasi agent bisa baca struktur folder project
- [ ] Verifikasi agent bisa identify pola berulang
- [ ] Verifikasi agent bisa buat pattern files di `patterns/project-specific/`
- [ ] Verifikasi agent bisa update `PATTERNS.md`
- **File terlibat:** (test scenario, tidak ada file diubah)
- **Dependency:** TASK-001 sampai TASK-011
- **Kompleksitas:** Medium
- **Estimasi:** 30 menit

### TASK-013: Test Workflow Pattern Control - Load Pattern Saat Coding
- [ ] Simulasi workflow: Agent mulai task coding dengan keyword "API"
- [ ] Verifikasi agent baca `PATTERNS.md`
- [ ] Verifikasi agent load pattern yang relevan (`api-handler.md` jika ada)
- [ ] Verifikasi agent ikuti pattern saat coding
- [ ] Verifikasi agent catat di `code-log.md` jika pattern tidak ada
- **File terlibat:** (test scenario, tidak ada file diubah)
- **Dependency:** TASK-012
- **Kompleksitas:** Medium
- **Estimasi:** 30 menit

### TASK-014: Test Workflow Proposal First - Task Besar
- [ ] Simulasi workflow: Agent terima task besar (multi-file, arsitektur baru)
- [ ] Verifikasi agent tentukan task butuh proposal
- [ ] Verifikasi agent load pattern yang relevan
- [ ] Verifikasi agent buat `context/proposal.md` dengan format OKF + LCS Contract
- [ ] Verifikasi agent set status → `draft` dan STOP
- [ ] Verifikasi agent tidak lanjut coding tanpa approval
- **File terlibat:** (test scenario, tidak ada file diubah)
- **Dependency:** TASK-012, TASK-013
- **Kompleksitas:** Medium
- **Estimasi:** 30 menit

### TASK-015: Test Workflow Proposal First - User Approval
- [ ] Simulasi workflow: User approve proposal
- [ ] Verifikasi agent baca proposal yang sudah approved
- [ ] Verifikasi agent implementasi sesuai proposal
- [ ] Verifikasi agent update `delta-handoff.md` dengan perubahan aktual
- [ ] Verifikasi agent update `code-log.md` dengan log implementasi
- [ ] Verifikasi agent dokumentasikan deviasi (jika ada)
- **File terlibat:** (test scenario, tidak ada file diubah)
- **Dependency:** TASK-014
- **Kompleksitas:** Medium
- **Estimasi:** 30 menit

### TASK-016: Test Edge Case - Pattern Konflik
- [ ] Simulasi workflow: Ada 2 pattern yang bertentangan
- [ ] Verifikasi agent tanya user: "Ada konflik antara pattern A dan B. Mana yang harus saya pakai?"
- [ ] Verifikasi agent tunggu user respond sebelum lanjut
- **File terlibat:** (test scenario, tidak ada file diubah)
- **Dependency:** TASK-013
- **Kompleksitas:** Simple
- **Estimasi:** 15 menit

### TASK-017: Test Edge Case - User Tidak Respond
- [ ] Simulasi workflow: Agent submit proposal, user tidak respond
- [ ] Verifikasi agent tunggu sampai user respond
- [ ] Verifikasi agent tidak lanjut coding tanpa approval
- [ ] Verifikasi agent tidak timeout atau error
- **File terlibat:** (test scenario, tidak ada file diubah)
- **Dependency:** TASK-014
- **Kompleksitas:** Simple
- **Estimasi:** 15 menit

---

## FASE 6: Documentation & Cleanup

### TASK-018: Update PATTERNS.md ke Status Final
- [ ] Baca `.lcsagent/PATTERNS.md`
- [ ] Ubah `lcs-status: "draft"` jadi `lcs-status: "final"`
- [ ] Verifikasi semua pattern di index sudah dibuat (minimal 2: proposal-first, tdd-workflow)
- [ ] Verifikasi semua pointer ke pattern files valid
- **File terlibat:** `.lcsagent/PATTERNS.md`
- **Dependency:** TASK-005, TASK-006, TASK-012 sampai TASK-017
- **Kompleksitas:** Simple
- **Estimasi:** 5 menit

### TASK-019: Buat Documentation - README Pattern Control
- [ ] Buat file `.lcsagent/docs/pattern-control-guide.md`
- [ ] Include: Cara pakai, workflow extract pattern, workflow load pattern, workflow maintenance pattern
- [ ] Include: Contoh konkret untuk setiap workflow
- [ ] Include: FAQ & troubleshooting
- **File terlibat:** `.lcsagent/docs/pattern-control-guide.md`
- **Dependency:** TASK-001 sampai TASK-018
- **Kompleksitas:** Medium
- **Estimasi:** 30 menit

### TASK-020: Buat Documentation - README Proposal First
- [ ] Buat file `.lcsagent/docs/proposal-first-guide.md`
- [ ] Include: Cara pakai, kriteria conditional, workflow step-by-step
- [ ] Include: Contoh konkret untuk setiap workflow
- [ ] Include: FAQ & troubleshooting
- **File terlibat:** `.lcsagent/docs/proposal-first-guide.md`
- **Dependency:** TASK-001 sampai TASK-018
- **Kompleksitas:** Medium
- **Estimasi:** 30 menit

### TASK-021: Update Changelog
- [ ] Baca `.lcsagent/CHANGELOG.md` (jika ada) atau buat baru
- [ ] Tambahkan entry untuk fitur Pattern Control
- [ ] Tambahkan entry untuk fitur Proposal First Execution
- [ ] Include: What's New, What's Changed, Breaking Changes (jika ada)
- **File terlibat:** `.lcsagent/CHANGELOG.md`
- **Dependency:** TASK-001 sampai TASK-020
- **Kompleksitas:** Simple
- **Estimasi:** 10 menit

---

## ACCEPTANCE CRITERIA CHECKLIST

### Pattern Control
- [ ] ✅ Folder `.lcsagent/patterns/` sudah dibuat (TASK-001)
- [ ] ✅ File `PATTERNS.md` (index) sudah dibuat dengan format OKF (TASK-004)
- [ ] ✅ Pattern files bisa dibuat dengan format OKF + LCS Contract (TASK-002)
- [ ] ✅ Agent bisa extract pattern dari codebase yang sudah ada (TASK-012)
- [ ] ✅ Agent bisa load pattern yang relevan saat coding (TASK-013)
- [ ] ✅ Agent bisa suggest update pattern setelah task selesai (TASK-007)
- [ ] ✅ Agent bisa buat pattern baru on-demand dari user request (TASK-012)
- [ ] ✅ Pattern yang dilanggar tanpa alasan = violation di `review.md` (TASK-008)

### Proposal First Execution
- [ ] ✅ Template proposal sudah dibuat di `templates/proposal.md` (TASK-003)
- [ ] ✅ Agent bisa tentukan apakah task butuh proposal atau tidak (TASK-014)
- [ ] ✅ Agent bisa buat proposal dengan format OKF + LCS Contract (TASK-014)
- [ ] ✅ Agent STOP setelah submit proposal (TASK-014)
- [ ] ✅ Agent bisa terima approval/rejection/request changes dari user (TASK-015)
- [ ] ✅ Agent implementasi sesuai proposal yang sudah approved (TASK-015)
- [ ] ✅ Agent dokumentasikan deviasi dari proposal (jika ada) di `code-log.md` (TASK-015)
- [ ] ✅ User bisa override kriteria default (TASK-007)

### Integrasi
- [ ] ✅ `AGENTS.md` sudah diupdate dengan workflow baru untuk `@lcs-coding` (TASK-007)
- [ ] ✅ `AGENTS.md` sudah diupdate dengan capability review proposal untuk `@lcs-review` (TASK-008)
- [ ] ✅ `context-pack.md` sudah diupdate dengan pointer ke proposal & pattern (TASK-010)
- [ ] ✅ `LCS-AGENT.md` (Source of Truth) sudah diupdate dengan section 3.4 & 3.5 (TASK-009)
- [ ] ✅ Workflow end-to-end bisa dijalankan tanpa error (TASK-012 sampai TASK-017)

---

## SUMMARY

**Total Task:** 21 task
**Total Fase:** 6 fase
**Estimasi Total Waktu:** ~5-6 jam (sekuensial)

**Critical Path:**
1. TASK-001 → TASK-002 → TASK-004 → TASK-005 → TASK-007 → TASK-009 → TASK-012 → TASK-014 → TASK-018

**Parallelizable:**
- TASK-002, TASK-003 bisa dikerjakan paralel
- TASK-005, TASK-006 bisa dikerjakan paralel
- TASK-007, TASK-008 bisa dikerjakan paralel
- TASK-019, TASK-020 bisa dikerjakan paralel

**Dependency Tree:**
```
FASE 1: Setup
├─ TASK-001 (folder)
├─ TASK-002 (template pattern) → TASK-005, TASK-006
├─ TASK-003 (template proposal) → TASK-008
└─ TASK-004 (PATTERNS.md) → TASK-005, TASK-006

FASE 2: Universal Pattern
├─ TASK-005 (proposal-first pattern) → TASK-007
└─ TASK-006 (tdd-workflow pattern) → TASK-007

FASE 3: Documentation Core
├─ TASK-007 (@lcs-coding workflow) → TASK-009, TASK-012
├─ TASK-008 (@lcs-review capability) → TASK-009
└─ TASK-009 (LCS-AGENT.md) → TASK-012

FASE 4: Context System
├─ TASK-010 (context-pack.md) → TASK-012
└─ TASK-011 (delta-handoff.md) → optional

FASE 5: Testing
├─ TASK-012 (test extract pattern) → TASK-013
├─ TASK-013 (test load pattern) → TASK-014, TASK-016
├─ TASK-014 (test proposal task besar) → TASK-015, TASK-017
├─ TASK-015 (test proposal approval) → TASK-018
├─ TASK-016 (test edge case konflik) → TASK-018
└─ TASK-017 (test edge case no respond) → TASK-018

FASE 6: Documentation & Cleanup
├─ TASK-018 (finalize PATTERNS.md) → TASK-019, TASK-020
├─ TASK-019 (doc pattern control) → TASK-021
├─ TASK-020 (doc proposal first) → TASK-021
└─ TASK-021 (changelog) → DONE
```

---

**Dibuat oleh**: @lcs-task  
**Tanggal**: 4 Juli 2026  
**Status**: Draft (menunggu review sebelum eksekusi)
