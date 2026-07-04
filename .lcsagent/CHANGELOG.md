# Changelog - LCS Agent

## [2.0.0] - 2026-07-04

### 🎉 What's New

#### Pattern Control
- ✅ Agent bisa belajar pola coding dari codebase yang sudah ada
- ✅ Agent otomatis load pattern berdasarkan keyword di task
- ✅ Pattern library dengan 2 kategori: Universal & Project-Specific
- ✅ Mekanisme extract pattern dari codebase (bootstrap)
- ✅ Mekanisme maintenance pattern (update otomatis)
- ✅ On-demand pattern loading (hemat token)

**Files Added:**
- `.lcsagent/PATTERNS.md` - Index pattern library
- `.lcsagent/patterns/universal/proposal-first.md` - Pattern kapan harus buat proposal
- `.lcsagent/patterns/universal/tdd-workflow.md` - Pattern workflow TDD
- `.lcsagent/templates/pattern.md` - Template untuk buat pattern baru
- `.lcsagent/docs/pattern-control-guide.md` - Dokumentasi lengkap Pattern Control

#### Proposal First Execution
- ✅ Agent membuat proposal sebelum coding (jika task besar/complex)
- ✅ Kriteria conditional (WAJIB vs TIDAK PERLU proposal)
- ✅ User bisa override kriteria default
- ✅ Workflow 5 step: Analisis → Buat Proposal → Wait Approval → Execute → Verify
- ✅ Proposal sebagai contract antara agent dan user
- ✅ Dokumentasi deviasi dari proposal (traceability)

**Files Added:**
- `.lcsagent/templates/proposal.md` - Template proposal implementasi
- `.lcsagent/docs/proposal-first-guide.md` - Dokumentasi lengkap Proposal First Execution

### 🔧 What's Changed

#### AGENTS.md
- Updated workflow `@lcs-coding` dengan 7 step baru (Analisis Task, Load Pattern, Buat Proposal, Wait for Approval, Execute Implementation, Verify & Document, Suggest Pattern Update)
- Updated workflow `@lcs-review` dengan capability review proposal (checklist: Completeness, Feasibility, Pattern Compliance, Scope, Risks, Test Plan)

#### LCS-AGENT.md (New)
- Source of truth untuk LCS Agent workflow
- Section 3.4: Pattern Control (struktur, aturan, trigger otomatis, maintenance)
- Section 3.5: Proposal First Execution (kriteria default, workflow)

#### Templates
- Added `context-pack.md` - Compact context untuk agent (hemat token)
- Added `delta-handoff.md` - Log perubahan per session (handoff ke user/agent lain)

### 🎯 Impact

**Hemat Token:**
- Pattern di-load on-demand (tidak semua pattern di-load sekaligus)
- Proposal mencegah trial-error (hemat waktu & token)
- Compact context method (baca file yang relevan saja)

**Kualitas Kode:**
- Kode lebih konsisten (mengikuti pola yang sudah ada)
- Kode lebih testable (TDD workflow pattern)
- Kode lebih traceable (proposal + deviasi terdokumentasi)

**User Control:**
- User bisa review proposal sebelum implementasi (mencegah salah arah)
- User bisa approve/reject/request changes (iterative refinement)
- User bisa override kriteria default (flexibility)

### 📦 Dependencies
- Opencode CLI
- Zed Editor
- Markdown

### 🚧 Breaking Changes
None. Backward compatible dengan LCS Agent v1.x.

### 📝 Migration Guide
Tidak perlu migration. LCS Agent v2.0 bisa digunakan langsung di project baru atau project existing.

**Untuk project existing:**
1. Copy folder `.lcsagent/` dari project baru
2. Run "Scan codebase dan buat pattern" untuk bootstrap pattern library
3. Review & approve pattern yang di-extract
4. Lanjut workflow seperti biasa

---

## [1.0.0] - 2026-06-01

### 🎉 Initial Release
- Basic LCS Agent workflow (explore, PRD, task, coding, review, debug, doc)
- Artifact contract (parent-child relationship)
- OKF + LCS Contract metadata
- Compact context method
- Anti-looping mechanism
- Chain of truth

---

**Format**: [Version] - YYYY-MM-DD
**Versioning**: Semantic Versioning (MAJOR.MINOR.PATCH)
