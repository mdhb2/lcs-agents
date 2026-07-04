---
# === OKF METADATA ===
type: "LCS-PATTERN"
title: "TDD Workflow"
description: "Pattern workflow TDD (Test-Driven Development) yang wajib diikuti agent"
resource: ".lcsagent/patterns/universal/tdd-workflow.md"
tags: [pattern, tdd, testing, workflow, universal]
timestamp: 2026-07-04T15:55:00Z
# === LCS CONTRACT METADATA ===
lcs-status: "final"
lcs-parent: "pattern-index"
lcs-children: []
lcs-session: "pattern-control"
---

# Pattern: TDD Workflow

## Kapan Dipakai
Agent `@lcs-coding` WAJIB mengikuti workflow TDD untuk:

**WAJIB TDD Jika:**
- Task involve logic bisnis (bukan UI/styling)
- Task involve API endpoint (backend)
- Task involve data processing/transformation
- Task involve critical business rule
- User explicitly bilang: "Pakai TDD" atau "Test first"

**OPTIONAL TDD Jika:**
- Task kecil tapi ada logic yang bisa di-test
- Task refactoring (test sudah ada, tapi perlu update)

**TIDAK PERLU TDD Jika:**
- Task pure UI/styling (tidak ada logic)
- Task dokumentasi (tidak ada kode)
- Task setup/konfigurasi (tidak ada logic bisnis)
- User explicitly bilang: "Skip test" atau "No test"

## Struktur/Template
Workflow TDD mengikuti siklus **Red-Green-Refactor**:

```
1. RED: Tulis test yang gagal
   └─> Test harus describe expected behavior
   └─> Run test → harus FAIL (karena implementation belum ada)

2. GREEN: Tulis implementation minimal untuk pass test
   └─> Implementation boleh jelek/hardcode (tujuan: pass test dulu)
   └─> Run test → harus PASS

3. REFACTOR: Refactor implementation (bersihkan kode)
   └─> Improve readability, remove duplication, optimize
   └─> Run test → harus tetap PASS
```

## Contoh Implementasi

### Contoh 1: TDD untuk API Endpoint
**Task:** "Buat API endpoint GET /users/:id untuk get user by ID"

**Step 1: RED (Tulis test yang gagal)**
```javascript
// test/users.test.js
describe('GET /users/:id', () => {
  it('should return user by id', async () => {
    const res = await request(app).get('/users/1');
    expect(res.status).toBe(200);
    expect(res.body).toHaveProperty('id', 1);
    expect(res.body).toHaveProperty('name');
  });

  it('should return 404 if user not found', async () => {
    const res = await request(app).get('/users/999');
    expect(res.status).toBe(404);
    expect(res.body).toHaveProperty('error');
  });
});
```

Run test: `npm test` → FAIL (endpoint belum ada)

**Step 2: GREEN (Tulis implementation minimal)**
```javascript
// routes/users.js
app.get('/users/:id', async (req, res) => {
  const user = await db.users.findById(req.params.id);
  if (!user) {
    return res.status(404).json({ error: 'User not found' });
  }
  res.json(user);
});
```

Run test: `npm test` → PASS

**Step 3: REFACTOR (Bersihkan kode)**
```javascript
// routes/users.js
app.get('/users/:id', async (req, res) => {
  try {
    const userId = parseInt(req.params.id, 10);
    const user = await userService.getById(userId);
    res.json(user);
  } catch (error) {
    if (error.name === 'NotFoundError') {
      return res.status(404).json({ error: error.message });
    }
    res.status(500).json({ error: 'Internal server error' });
  }
});
```

Run test: `npm test` → PASS (refactor berhasil, test tetap hijau)

### Contoh 2: TDD untuk Business Logic
**Task:** "Buat fungsi calculateDiscount untuk hitung diskon berdasarkan total & membership tier"

**Step 1: RED**
```javascript
// test/discount.test.js
describe('calculateDiscount', () => {
  it('should return 10% discount for bronze member', () => {
    const discount = calculateDiscount(100, 'bronze');
    expect(discount).toBe(10);
  });

  it('should return 20% discount for silver member', () => {
    const discount = calculateDiscount(100, 'silver');
    expect(discount).toBe(20);
  });

  it('should return 30% discount for gold member', () => {
    const discount = calculateDiscount(100, 'gold');
    expect(discount).toBe(30);
  });

  it('should return 0 discount for non-member', () => {
    const discount = calculateDiscount(100, null);
    expect(discount).toBe(0);
  });
});
```

Run test: `npm test` → FAIL (fungsi belum ada)

**Step 2: GREEN**
```javascript
// services/discount.js
function calculateDiscount(total, tier) {
  if (tier === 'bronze') return total * 0.1;
  if (tier === 'silver') return total * 0.2;
  if (tier === 'gold') return total * 0.3;
  return 0;
}
```

Run test: `npm test` → PASS

**Step 3: REFACTOR**
```javascript
// services/discount.js
const DISCOUNT_RATES = {
  bronze: 0.1,
  silver: 0.2,
  gold: 0.3,
};

function calculateDiscount(total, tier) {
  const rate = DISCOUNT_RATES[tier] || 0;
  return total * rate;
}
```

Run test: `npm test` → PASS

## Anti-Pattern (JANGAN Lakukan)
- ❌ **Tulis implementation dulu, baru test** - Bukan TDD, ini testing after the fact
- ❌ **Skip RED step (test langsung pass)** - Test tidak validate behavior
- ❌ **Test yang terlalu general/vague** - Tidak jelas expected behavior
- ❌ **Implementation yang terlalu complex di GREEN step** - Refactor dulu baru optimize
- ❌ **Refactor tanpa run test** - Risiko break functionality

## Checklist
Agent WAJIB cek item ini sebelum finalize TDD workflow:

### Saat RED Step
- [ ] Test sudah ditulis dengan jelas (describe expected behavior)
- [ ] Test sudah di-run → FAIL (karena implementation belum ada)
- [ ] Test coverage mencakup happy path & edge case
- [ ] Test tidak tergantung external dependencies (pakai mock jika perlu)

### Saat GREEN Step
- [ ] Implementation minimal sudah ditulis (tujuan: pass test)
- [ ] Test sudah di-run → PASS
- [ ] Implementation boleh jelek/hardcode (akan diperbaiki di REFACTOR)

### Saat REFACTOR Step
- [ ] Implementation sudah di-refactor (improve readability, remove duplication)
- [ ] Test sudah di-run → tetap PASS
- [ ] No new feature added (fokus: bersihkan kode)

### Saat Task Selesai
- [ ] Semua test PASS
- [ ] Code coverage cukup (minimal happy path & 1-2 edge case)
- [ ] Test sudah di-commit bersama implementation
- [ ] `code-log.md` sudah diupdate dengan log test

## Catatan
- TDD menghemat waktu debugging (test catch bug lebih awal)
- TDD improve code quality (code lebih testable, less coupling)
- TDD adalah dokumentasi live (test describe expected behavior)
- Jangan perfectionist di GREEN step (refactor nanti di REFACTOR step)
- Test harus fast (jangan pakai real database, pakai in-memory atau mock)
