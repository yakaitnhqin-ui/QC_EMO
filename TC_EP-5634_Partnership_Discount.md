# Test Cases — EP-5634: [Partnership] Allow fixed discount for merchant

> **Jira ticket**: [EP-5634](https://eskimo-portal.atlassian.net/browse/EP-5634)
> **Summary**: [Partnership] Allow fixed discount for merchant — Refactor Discount Benefit field
> **Status hiện tại**: QC | **Sprint**: Sprint 95
> **Priority**: High | **Tester**: Nhu Huynh | **Ngày**: 2026-05-28

---

## Tổng quan tính năng

Refactor **Discount Benefit** trong mục Merchant của Partner detail (Admin Portal) để hỗ trợ 3 loại discount:

| Option | Mô tả | Ghi chú |
|--------|-------|---------|
| **Fixed Discount (US$)** | Giảm một số tiền cố định | **Mới thêm** |
| **Percentage (%)** | Giảm theo phần trăm | Merchant hiện tại đang dùng option này |
| **No Discount** | Không áp dụng discount | Default khi tạo mới |

**Phạm vi thay đổi:**
- **AC1 — Merchant section**: Đổi tên fields/buttons, thêm 3 options discount, hiển thị cảnh báo (giống Corporate program EP-2810), cập nhật voucher pricing, refactor UI Generate Voucher ở Partner Portal
- **AC2 — Corporate section**: Đổi tên fields/buttons cùng convention

**Locations:**
- Admin Portal → Partnership → Partner detail → **Merchant section** → Discount Benefit (AC1)
- Admin Portal → Partnership → Partner detail → **Corporate section** (AC2)
- Partner Portal → Partner Sale → **Generate Voucher** (AC1 — voucher UI)

---

## APIs liên quan (đối chiếu WBS)

| # | API | Method | Mục đích |
|---|-----|--------|---------|
| 1 | `/api/partner/v2/{partnerId}` | GET | Lấy partner detail — đọc discount benefit hiện tại |
| 2 | `/api/partner/v2` | PUT | Cập nhật partner — lưu discount benefit mới |
| 3 | `/api/partner/v2` | POST | Tạo partner mới — gắn discount mặc định (No Discount) |
| 4 | `/api/data-plan/get-discount-price-for-dataplans` | POST | Tính giá voucher sau khi discount được áp dụng |
| 5 | `/api/voucher` | POST | Tạo voucher (Partner portal) — phản ánh discounted price |
| 6 | `/api/partner/search` | POST | Tìm kiếm partner/merchant để verify |

---

## Danh sách Test Case

| TC ID | Tên | Type | Priority | AC |
|-------|-----|------|----------|----|
| TC-EP-PDSC-001 | Verify UI naming — Merchant section fields đổi đúng tên | Happy path | P1 | AC1 |
| TC-EP-PDSC-002 | Tạo mới Discount Benefit — default là "No Discount" | Happy path | P1 | AC1 |
| TC-EP-PDSC-003 | Thêm Discount Benefit — chọn Fixed Discount (US$) và lưu | Happy path | P1 | AC1 |
| TC-EP-PDSC-004 | Thêm Discount Benefit — chọn Percentage (%) và lưu | Happy path | P1 | AC1 |
| TC-EP-PDSC-005 | Fixed discount áp dụng đúng vào voucher price ở Partner Portal | Happy path | P1 | AC1 |
| TC-EP-PDSC-006 | Percentage discount áp dụng đúng vào voucher price ở Partner Portal | Happy path | P1 | AC1 |
| TC-EP-PDSC-007 | No Discount — voucher hiển thị nguyên giá gốc, không trừ | Happy path | P1 | AC1 |
| TC-EP-PDSC-008 | Generate Voucher UI — format hiển thị Discount field đúng chuẩn mới | Happy path | P1 | AC1 |
| TC-EP-PDSC-009 | Verify UI naming — Corporate section fields đổi đúng tên (AC2) | Happy path | P1 | AC2 |
| TC-EP-PDSC-010 | Warning hiển thị khi thay đổi loại discount | Negative | P1 | AC1 |
| TC-EP-PDSC-011 | Fixed Discount = 0 US$ — behavior xử lý đúng | Negative | P2 | AC1 |
| TC-EP-PDSC-012 | Fixed Discount > giá gốc voucher — validation error | Negative | P1 | AC1 |
| TC-EP-PDSC-013 | Merchant hiện tại (đang dùng Percentage %) — data preserved sau refactor | Edge | P1 | AC1 |
| TC-EP-PDSC-014 | Nhiều merchants — discount settings độc lập nhau | Edge | P2 | AC1 |
| TC-EP-PDSC-015 | Xóa Discount Benefit → voucher revert về giá gốc | Edge | P2 | AC1 |
| TC-EP-PDSC-016 | Chuyển từ Percentage sang Fixed — cảnh báo + voucher price cập nhật | Edge | P1 | AC1 |

---

## Chi tiết Test Case

---

### TC-EP-PDSC-001 | Verify UI naming — Merchant section fields đổi đúng tên

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Partner detail, Merchant section |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Functional / UI verification |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Đã có partner/merchant tồn tại trong hệ thống
- Đăng nhập Admin Portal với quyền Partnership

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Admin Portal → Partnership | — |
| 2 | Chọn một partner có merchant section | Merchant partner ID: `[test-merchant-id]` |
| 3 | Mở Partner detail → quan sát Merchant section | — |
| 4 | Kiểm tra tên button thêm mới | — |
| 5 | Kiểm tra tên section/card title | — |
| 6 | Kiểm tra tên label của subfield discount | — |

**Expected Result:**
- Bước 4: Button tên là **"New Discount Benefit"** (không phải tên cũ)
- Bước 5: Section/card title là **"Discount Benefit"**
- Bước 6: Label subfield là **"Discount"**

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PDSC-002 | Tạo mới Discount Benefit — default là "No Discount"

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — New Discount Benefit, default state |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Merchant chưa có Discount Benefit nào
- Đăng nhập Admin Portal

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner detail → Merchant section | — |
| 2 | Click **"New Discount Benefit"** | — |
| 3 | Quan sát form/dialog mới mở ra | — |
| 4 | Kiểm tra giá trị mặc định của dropdown **"Discount"** | — |
| 5 | Không thay đổi gì, click **Save/Confirm** | — |

**Expected Result:**
- Bước 3: Form/dialog tạo mới Discount Benefit hiển thị
- Bước 4: Dropdown **"Discount"** mặc định chọn **"No Discount"** (không phải Fixed hay Percentage)
- Bước 5: Lưu thành công — record Discount Benefit tạo với type = No Discount

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PDSC-003 | Thêm Discount Benefit — chọn Fixed Discount (US$) và lưu

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Discount Benefit, Fixed Discount |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `PUT /api/partner/v2`, `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Merchant đang có Discount Benefit (hoặc tạo mới từ TC-002)
- Đăng nhập Admin Portal

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner detail → Merchant section → Discount Benefit | — |
| 2 | Mở edit hoặc tạo mới | — |
| 3 | Dropdown **"Discount"** → chọn **"Fixed Discount (US$)"** | — |
| 4 | Nhập giá trị discount | `1.00` USD |
| 5 | Click **Save** | — |
| 6 | Reload trang, mở lại Partner detail | — |
| 7 | Verify giá trị đã lưu | — |

**Expected Result:**
- Bước 3: Option **"Fixed Discount (US$)"** có thể chọn được trong dropdown
- Bước 4: Input nhận giá trị dạng số thập phân USD
- Bước 5: `PUT /api/partner/v2` gọi thành công, trả về 200
- Bước 6: Discount hiển thị đúng là **"Fixed Discount (US$)"** với giá trị `1.00`
- Bước 7: Dữ liệu được persist sau reload

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PDSC-004 | Thêm Discount Benefit — chọn Percentage (%) và lưu

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Discount Benefit, Percentage |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `PUT /api/partner/v2`, `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Merchant với Discount Benefit có thể edit

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner detail → Merchant section → Discount Benefit → Edit | — |
| 2 | Dropdown **"Discount"** → chọn **"Percentage (%)"** | — |
| 3 | Nhập giá trị phần trăm | `10` (%) |
| 4 | Click **Save** | — |
| 5 | Reload trang, verify | — |

**Expected Result:**
- Bước 2: Option **"Percentage (%)"** có thể chọn được
- Bước 3: Input nhận giá trị số (0–100)
- Bước 4: Lưu thành công — API trả 200
- Bước 5: Discount hiển thị **"Percentage (%)"** = `10%`

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-PDSC-005 | Fixed discount áp dụng đúng vào voucher price ở Partner Portal

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership → B Partner Portal — Voucher pricing |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path / Integration |
| **API** | `POST /api/data-plan/get-discount-price-for-dataplans`, `POST /api/voucher` |

**Precondition:**
- Merchant đã được gán **Fixed Discount = 1.00 US$** (từ TC-003)
- Voucher có Original Price = **6.00 US$**
- Đăng nhập Partner Portal với account thuộc merchant đó

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner Portal → Partner Sale → Generate Voucher | — |
| 2 | Chọn data plan có giá gốc **6.00 US$** | Plan: `[test-plan-6usd]` |
| 3 | Quan sát giá hiển thị trong form generate | — |
| 4 | Kiểm tra giá partner phải trả (sau discount) | Expected: 6.00 - 1.00 = **5.00 US$** |
| 5 | Hoàn thành generate voucher | — |
| 6 | Kiểm tra voucher record được tạo ra | — |

**Expected Result:**
- Bước 3: Hiển thị **Original price: 6.00 US$**, **Discount: 1.00 US$**
- Bước 4: Giá merchant trả = **5.00 US$** (giá sau fixed discount)
- Bước 5: `POST /api/voucher` gọi với discounted amount đúng
- Bước 6: Voucher record ghi nhận giá đã discount

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** Đây là test tích hợp quan trọng nhất của AC1.

---

### TC-EP-PDSC-006 | Percentage discount áp dụng đúng vào voucher price

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership → B Partner Portal — Voucher pricing |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path / Integration |
| **API** | `POST /api/data-plan/get-discount-price-for-dataplans`, `POST /api/voucher` |

**Precondition:**
- Merchant đã được gán **Percentage Discount = 10%**
- Voucher có Original Price = **6.00 US$**

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner Portal → Partner Sale → Generate Voucher | — |
| 2 | Chọn data plan có giá gốc **6.00 US$** | Plan: `[test-plan-6usd]` |
| 3 | Quan sát giá hiển thị | — |
| 4 | Kiểm tra giá partner trả | Expected: 6.00 × (1 - 10%) = **5.40 US$** |
| 5 | Generate voucher, kiểm tra record | — |

**Expected Result:**
- Bước 3: Hiển thị **Original price: 6.00 US$**, **Discount: 10%** (hoặc 0.60 US$)
- Bước 4: Giá merchant trả = **5.40 US$**
- Bước 5: Voucher record ghi nhận đúng discounted amount

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-PDSC-007 | No Discount — voucher hiển thị nguyên giá gốc

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership → B Partner Portal — No discount |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `POST /api/voucher` |

**Precondition:**
- Merchant có Discount Benefit = **"No Discount"**
- Voucher Original Price = **6.00 US$**

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Đăng nhập Partner Portal → Partner Sale → Generate Voucher | — |
| 2 | Chọn data plan giá 6.00 US$ | — |
| 3 | Quan sát Discount field | — |
| 4 | Kiểm tra giá merchant phải trả | Expected: **6.00 US$** (không có discount) |
| 5 | Generate voucher, verify | — |

**Expected Result:**
- Bước 3: Discount hiển thị là **"No discount"** (không có số tiền)
- Bước 4: Giá = **6.00 US$** — bằng giá gốc, không bị trừ bất kỳ khoản nào
- Bước 5: Voucher record ghi nhận đúng full price

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-PDSC-008 | Generate Voucher UI — Discount field hiển thị đúng format mới

| Field | Nội dung |
|-------|---------|
| **Module** | B Partner Portal — Generate Voucher, UI refactor |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | UI / Functional |
| **API** | `POST /api/data-plan/get-discount-price-for-dataplans` |

**Precondition:**
- Chuẩn bị 3 merchant accounts với 3 loại discount khác nhau:
  - Merchant A: Fixed Discount = 1.00 US$
  - Merchant B: Percentage = 1%
  - Merchant C: No Discount

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Login với Merchant A → Generate Voucher → quan sát Discount field | Plan: 6.00 US$ |
| 2 | Login với Merchant B → Generate Voucher → quan sát Discount field | Plan: 6.00 US$ |
| 3 | Login với Merchant C → Generate Voucher → quan sát Discount field | Plan: 6.00 US$ |

**Expected Result:**
- Bước 1 (Fixed): Discount hiển thị dạng **"1.00 US$"** (không phải %)
- Bước 2 (Percentage): Discount hiển thị dạng **"1.00 %"** (không phải US$)
- Bước 3 (No Discount): Discount hiển thị **"No discount"**
- Tất cả: **Original price: 6.00 US$** hiển thị đúng và nhất quán

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** Verify format text chính xác theo AC1: `1.00 %` / `6.00 US$` / `"No discount"`.

---

### TC-EP-PDSC-009 | Verify UI naming — Corporate section đổi đúng tên (AC2)

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Partner detail, Corporate section |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | UI verification |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Partner có cả Corporate section
- Đăng nhập Admin Portal

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Admin Portal → Partnership → Partner detail | Partner có Corporate section |
| 2 | Quan sát Corporate section | — |
| 3 | Kiểm tra button thêm mới | — |
| 4 | Kiểm tra section/card title | — |
| 5 | Kiểm tra label của subfield | — |

**Expected Result:**
- Bước 3: Button là **"New Discount Benefit"**
- Bước 4: Section title là **"Discount Benefit"**
- Bước 5: Subfield label là **"Discount"**
- Tất cả: Nhất quán với naming ở Merchant section (AC1)

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-PDSC-010 | Warning hiển thị khi thay đổi loại discount

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Discount Benefit, change warning |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Negative / UI behavior |
| **API** | `PUT /api/partner/v2` |

**Precondition:**
- Merchant đang có Discount Benefit = **Percentage (%) = 10%**
- Refer: warning behavior của Corporate program (EP-2810)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner detail → Merchant → Discount Benefit → Edit | — |
| 2 | Thay đổi **Discount** dropdown từ **"Percentage"** sang **"Fixed Discount (US$)"** | — |
| 3 | Quan sát có cảnh báo không | — |
| 4 | Điền giá trị mới | `2.00` USD |
| 5 | Confirm cảnh báo và Save | — |
| 6 | Reload, verify discount type mới | — |

**Expected Result:**
- Bước 3: Warning dialog/message xuất hiện (tương tự Corporate program EP-2810), nội dung cảnh báo rằng việc thay đổi discount sẽ ảnh hưởng đến giá voucher
- Bước 5: Sau khi confirm → `PUT /api/partner/v2` gọi thành công
- Bước 6: Discount type đã đổi sang Fixed Discount = 2.00 US$

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note:** Cần confirm với Dev/BA nội dung exact của warning message để verify đúng text.

---

### TC-EP-PDSC-011 | Fixed Discount = 0 US$ — behavior xử lý đúng

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Discount Benefit, edge value |
| **Jira ref** | EP-5634 |
| **Priority** | P2 — High |
| **Type** | Negative / Boundary |
| **API** | `PUT /api/partner/v2` |

**Precondition:**
- Merchant có Discount Benefit đang edit

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Discount Benefit → chọn **Fixed Discount (US$)** | — |
| 2 | Nhập giá trị **0** | `0` hoặc `0.00` |
| 3 | Click **Save** | — |
| 4 | Kiểm tra voucher pricing trong Partner Portal | — |

**Expected Result:**
- Bước 3: Hệ thống xử lý hợp lệ — **hoặc** (a) lưu được và treat như No Discount, **hoặc** (b) validation error "Discount must be greater than 0"
- Bước 4: Nếu lưu được → voucher price = original price (0 discount không trừ gì)
- **Cần xác nhận với Dev/BA** behavior kỳ vọng cho input 0

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note:** Cần confirm với BA: Fixed Discount = 0 có hợp lệ không, hay phải chọn "No Discount"?

---

### TC-EP-PDSC-012 | Fixed Discount > giá gốc voucher — validation error

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Discount Benefit, invalid amount |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Negative / Validation |
| **API** | `PUT /api/partner/v2`, `POST /api/voucher` |

**Precondition:**
- Voucher original price = **6.00 US$**
- Merchant được set Fixed Discount

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Set Fixed Discount = **10.00 US$** (lớn hơn giá gốc 6.00) | `10.00` |
| 2 | Click **Save** trên Admin Portal | — |
| 3 | **Test path B**: Admin lưu thành công, vào Partner Portal → Generate Voucher | — |
| 4 | Quan sát giá sau discount | — |

**Expected Result:**
- **Path A (ideal)**: Admin Portal validate và từ chối lưu nếu fixed discount > một ngưỡng nào đó
- **Path B (nếu backend không validate tại save)**: Partner Portal → Generate Voucher → giá sau discount không âm (floor at 0 hoặc hiển thị "Free")
- Không được hiển thị giá âm (e.g., -4.00 US$)

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note:** Cần xác nhận với Dev/BA: hệ thống có validate fixed discount không vượt original price không? Hay floor về 0?

---

### TC-EP-PDSC-013 | Merchant hiện tại đang dùng Percentage — data preserved sau refactor

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Migration / backward compatibility |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Edge / Regression |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Có merchant **đã tồn tại từ trước** khi deploy tính năng này, với Percentage discount (ví dụ: 15%)
- Đây là "existing merchants" mà ticket đề cập

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Sau khi deploy EP-5634, vào Partner detail của merchant cũ | Merchant có Percentage discount cũ |
| 2 | Quan sát Discount Benefit | — |
| 3 | Kiểm tra loại discount được chọn | — |
| 4 | Kiểm tra giá trị phần trăm | — |
| 5 | Vào Partner Portal → Generate Voucher với merchant này | — |
| 6 | Kiểm tra voucher pricing | — |

**Expected Result:**
- Bước 3: Loại discount vẫn là **"Percentage (%)"** (không bị reset về No Discount)
- Bước 4: Giá trị % vẫn đúng như cũ (ví dụ 15%)
- Bước 5–6: Voucher pricing vẫn tính đúng theo Percentage discount cũ
- Không có data loss, không có side effect sau migration

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note:** Đây là regression test quan trọng nhất — existing merchants phải không bị ảnh hưởng.

---

### TC-EP-PDSC-014 | Nhiều merchants — discount settings độc lập nhau

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Multiple merchants isolation |
| **Jira ref** | EP-5634 |
| **Priority** | P2 — High |
| **Type** | Edge / Isolation |
| **API** | `GET /api/partner/v2/{partnerId}`, `PUT /api/partner/v2` |

**Precondition:**
- Chuẩn bị ít nhất 2 merchants:
  - Merchant X: Fixed Discount = 1.00 US$
  - Merchant Y: Percentage = 20%

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Thay đổi discount của **Merchant X** thành Fixed = 2.00 US$ | Merchant X |
| 2 | Mở **Merchant Y**, kiểm tra discount | Merchant Y |
| 3 | Verify Merchant Y không bị ảnh hưởng | — |
| 4 | Login Partner Portal với Merchant X → Generate Voucher | Plan: 6.00 US$ |
| 5 | Login Partner Portal với Merchant Y → Generate Voucher | Plan: 6.00 US$ |

**Expected Result:**
- Bước 2–3: Merchant Y vẫn là Percentage = 20%, **không bị thay đổi**
- Bước 4: Voucher price của Merchant X = 6.00 - 2.00 = **4.00 US$**
- Bước 5: Voucher price của Merchant Y = 6.00 × 0.80 = **4.80 US$**
- Mỗi merchant có discount riêng, độc lập hoàn toàn

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-PDSC-015 | Xóa Discount Benefit → voucher revert về giá gốc

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Delete discount benefit |
| **Jira ref** | EP-5634 |
| **Priority** | P2 — High |
| **Type** | Edge / Delete behavior |
| **API** | `PUT /api/partner/v2`, `POST /api/voucher` |

**Precondition:**
- Merchant có Fixed Discount = 1.00 US$ đang active

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Admin Portal → Merchant section → Discount Benefit | — |
| 2 | Xóa hoặc deactivate Discount Benefit | — |
| 3 | Lưu lại | — |
| 4 | Vào Partner Portal → Generate Voucher | Plan: 6.00 US$ |
| 5 | Quan sát voucher pricing | — |

**Expected Result:**
- Bước 3: Xóa thành công
- Bước 4–5: Discount field hiển thị **"No discount"**, giá = **6.00 US$** (original, không trừ gì)
- Không còn tính fixed discount 1.00 US$ nữa

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-PDSC-016 | Chuyển Percentage → Fixed Discount — warning + voucher price cập nhật ngay

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Discount type change, full flow |
| **Jira ref** | EP-5634 |
| **Priority** | P1 — Critical |
| **Type** | Edge / State transition |
| **API** | `PUT /api/partner/v2`, `POST /api/data-plan/get-discount-price-for-dataplans` |

**Precondition:**
- Merchant đang có **Percentage Discount = 10%**
- Voucher original price test = **6.00 US$**
- Có account Partner Portal để verify

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Admin Portal → Merchant section → Discount Benefit → Edit | — |
| 2 | Đổi discount type từ **Percentage** sang **Fixed Discount (US$)** | — |
| 3 | Quan sát warning message | — |
| 4 | Confirm warning | — |
| 5 | Nhập **2.00 US$** → Save | `2.00` |
| 6 | Login Partner Portal với merchant này | — |
| 7 | Generate Voucher với plan 6.00 US$ | Plan: `[test-plan-6usd]` |
| 8 | Verify discount applied | — |

**Expected Result:**
- Bước 3: Warning xuất hiện (giống EP-2810 Corporate)
- Bước 5: Save thành công — type chuyển sang Fixed
- Bước 7: Discount field hiển thị **"2.00 US$"** (không còn hiển thị %)
- Bước 8: Voucher price = 6.00 - 2.00 = **4.00 US$** (không còn tính theo 10%)
- Thay đổi có hiệu lực **ngay lập tức** sau khi save, không cần action thêm

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** End-to-end test kết hợp warning + save + pricing effect.

---

## Tóm tắt coverage theo AC

| AC | Scenario | TC ID | Status |
|----|----------|-------|--------|
| AC1 | UI naming — Merchant section | TC-PDSC-001 | |
| AC1 | New Discount Benefit — default No Discount | TC-PDSC-002 | |
| AC1 | Add Fixed Discount (US$) | TC-PDSC-003 | |
| AC1 | Add Percentage (%) | TC-PDSC-004 | |
| AC1 | Fixed discount → voucher pricing correct | TC-PDSC-005 | |
| AC1 | Percentage discount → voucher pricing correct | TC-PDSC-006 | |
| AC1 | No Discount → full original price | TC-PDSC-007 | |
| AC1 | Generate Voucher UI — new format | TC-PDSC-008 | |
| AC2 | UI naming — Corporate section | TC-PDSC-009 | |
| AC1 | Warning on discount change | TC-PDSC-010 | |
| AC1 | Edge: Fixed Discount = 0 | TC-PDSC-011 | |
| AC1 | Negative: Fixed Discount > original price | TC-PDSC-012 | |
| AC1 | Regression: existing Percentage merchant preserved | TC-PDSC-013 | |
| AC1 | Isolation: multiple merchants independent | TC-PDSC-014 | |
| AC1 | Delete discount → revert to full price | TC-PDSC-015 | |
| AC1 | Transition Percentage → Fixed, end-to-end | TC-PDSC-016 | |

**Tổng: 16 test cases** | Happy path: 9 | Negative: 3 | Edge: 4

---

## Câu hỏi cần confirm với Dev/BA trước khi test

> ⚠️ Liên hệ confirm trước khi thực hiện test để tránh assumption sai.

1. **Tên cũ của fields** trước khi đổi là gì? (để verify đã đổi đúng — TC-001, TC-009)
2. **Warning message** chính xác khi thay đổi discount type là gì? Text cụ thể? (TC-010, TC-016)
3. **Fixed Discount = 0** có hợp lệ không? Xử lý như "No Discount" hay validation error? (TC-011)
4. **Fixed Discount > original price** — hệ thống validate tại save hay tại generate voucher? Floor về 0 hay error? (TC-012)
5. Sau khi đổi discount type, Partner Portal có cần **refresh/reload** để thấy giá mới không? (TC-016)
6. **Existing merchants** hiện đang có discount type gì trong DB (trước khi deploy)? (TC-013)
7. Discount Benefit có hỗ trợ **nhiều record** cùng lúc không, hay chỉ 1 active tại một thời điểm?
8. **Original price** trong Generate Voucher UI — là field read-only hay editable? (TC-008)

---

*Test Cases v1.0 — EP-5634 | Eskimo Portals Sprint 95 | 2026-05-28 | Tester: Nhu Huynh*
