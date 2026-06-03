# Test Cases — EP-5680: [Lâm][Admin] Add Total Balance section for Link-based & Network Affiliate in Partner Detail

> **Jira ticket**: [EP-5680](https://eskimo-portal.atlassian.net/browse/EP-5680)
> **Summary**: Add Total Balance section for Link-based & Network Affiliate in Partner Detail
> **Status hiện tại**: QC | **Sprint**: Current Sprint
> **Priority**: Medium | **Tester**: Nhu Huynh | **Ngày**: 2026-06-02
> **Environment**: Staging only
> **BE ticket**: [EA-4008](https://eskimoapi.atlassian.net/browse/EA-4008)

---

## Tổng quan tính năng

Thêm section **Total Balance** trong trang **Partner Detail** (Admin Portal) hiển thị available balance của partner:
- **Link-based Affiliate Balance**
- **Network Affiliate Balance**

> ⚠️ Feature chỉ available trên **Staging environment**.

---

## APIs liên quan

| # | API | Method | Mục đích |
|---|-----|--------|----------|
| 1 | `/api/partner/v2/{partnerId}` | GET | Lấy partner detail — bao gồm balance data |
| 2 | `/api/partner/search` | POST | Tìm kiếm partner theo type |
| 3 | `/api/commission/partner/{partnerId}` | GET | Lấy commission data để đối chiếu balance |

---

## Danh sách Test Case

| TC ID | Tên | Type | Priority | AC |
|-------|-----|------|----------|-----|
| TC-EP-PTN-001 | Verify Total Balance section hiển thị trên Partner Detail | Happy path | P1 | AC1 |
| TC-EP-PTN-002 | Verify Link-based Affiliate Balance hiển thị chính xác | Happy path | P1 | AC1 |
| TC-EP-PTN-003 | Verify Network Affiliate Balance hiển thị chính xác | Happy path | P1 | AC1 |
| TC-EP-PTN-004 | Verify cả 2 balance hiển thị khi partner có cả 2 loại commission | Happy path | P1 | AC1 |
| TC-EP-PTN-005 | Verify UI khớp với design reference | Happy path | P1 | AC1 |
| TC-EP-PTN-006 | Verify Balance cập nhật sau khi có commission mới | Happy path | P1 | AC1 |
| TC-EP-PTN-007 | Verify Balance giảm sau payment thành công | Happy path | P1 | AC1 |
| TC-EP-PTN-008 | Verify Balance = 0 khi Partner chưa có commission | Negative | P2 | AC1 |
| TC-EP-PTN-009 | Verify Balance không hiển thị cho partner type khác (Referral, v.v.) | Negative | P1 | AC1 |
| TC-EP-PTN-010 | Verify Balance không bị âm khi payment > commission | Negative | P1 | AC1 |
| TC-EP-PTN-011 | Verify Balance không bao gồm commission types khác | Negative | P1 | AC1 |
| TC-EP-PTN-012 | Verify Balance với giá trị lớn — format số liệu | Edge | P2 | AC1 |
| TC-EP-PTN-013 | Verify Balance khi nhiều partners — data isolation | Edge | P2 | AC1 |
| TC-EP-PTN-014 | Verify Balance sau khi payment fail — rollback đúng | Edge | P1 | AC1 |
| TC-EP-PTN-015 | Verify API response time cho Total Balance section | Non-functional | P2 | AC1 |
| TC-EP-PTN-016 | Verify không có console error khi load section | Non-functional | P2 | AC1 |

---

## Chi tiết Test Case

---

### TC-EP-PTN-001 | Verify Total Balance section hiển thị trên Partner Detail

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Partner Detail |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Đăng nhập Admin Portal trên **Staging** với account có quyền Partnership.
- Có Partner thuộc loại Link-based Affiliate hoặc Network Affiliate.
- Partner đã có commission data.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Truy cập Admin Portal → Partnership | URL: `https://[staging-domain]/partnership` |
| 2 | Chọn một Partner thuộc loại Link-based hoặc Network Affiliate | Partner ID: `[test-partner-id]` |
| 3 | Cuộn trang Partner Detail, tìm section **Total Balance** | — |
| 4 | Quan sát layout, vị trí section | — |
| 5 | Mở DevTools → Network tab, kiểm tra API call | — |

**Expected Result:**
- Bước 3: Section **Total Balance (Link-based & Network Affiliate)** hiển thị trên trang Partner Detail.
- Bước 4: Section hiển thị đúng vị trí theo UI reference (attachment `image-20260526-034313.png`).
- Bước 5: API `GET /api/partner/v2/{partnerId}` trả về 200, response body chứa balance data.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-002 | Verify Link-based Affiliate Balance hiển thị chính xác

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Partner Detail, Total Balance |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Partner loại **Link-based Affiliate** có commission data và payment history.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner Detail của partner Link-based Affiliate | Partner ID: `[link-based-id]` |
| 2 | Xem giá trị **Link-based Affiliate Balance** trong section Total Balance | — |
| 3 | Mở Commission Payment, tính thủ công: tổng commission earned - tổng payment thành công | — |
| 4 | Đối chiếu giá trị balance UI vs tính toán thủ công | — |
| 5 | Kiểm tra API response, so sánh field balance trong response | DevTools → Network tab |

**Expected Result:**
- Bước 2: Hiển thị dòng **Link-based Affiliate Balance** với giá trị số dư cụ thể.
- Bước 4: Giá trị balance = Tổng commission earned − Tổng payment thành công. Sai lệch = 0.
- Bước 5: Giá trị trên UI khớp với giá trị trong API response.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-003 | Verify Network Affiliate Balance hiển thị chính xác

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Partner Detail, Total Balance |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Partner loại **Network Affiliate** có commission data và payment history.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner Detail của partner Network Affiliate | Partner ID: `[network-affiliate-id]` |
| 2 | Xem giá trị **Network Affiliate Balance** trong section Total Balance | — |
| 3 | Tính thủ công: tổng commission earned − tổng payment thành công | — |
| 4 | Đối chiếu balance UI vs tính toán thủ công | — |
| 5 | Kiểm tra API response field balance | DevTools → Network tab |

**Expected Result:**
- Bước 2: Hiển thị dòng **Network Affiliate Balance** với giá trị số dư cụ thể.
- Bước 4: Balance = Tổng commission earned − Tổng payment thành công. Chính xác 100%.
- Bước 5: UI khớp API response.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-004 | Verify cả 2 balance hiển thị khi partner có cả 2 loại commission

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Partner Detail, Total Balance |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Partner vừa là Link-based Affiliate vừa là Network Affiliate.
- Có commission data cho cả 2 loại.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner Detail của partner có cả 2 loại affiliate | Partner ID: `[dual-type-id]` |
| 2 | Kiểm tra section Total Balance | — |
| 3 | Xác nhận cả 2 dòng balance đều hiển thị | — |
| 4 | Verify giá trị của từng loại balance độc lập | — |

**Expected Result:**
- Bước 2–3: Hiển thị cả **Link-based Affiliate Balance** và **Network Affiliate Balance** riêng biệt.
- Bước 4: Mỗi loại balance tính toán độc lập, không gộp chung.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-005 | Verify UI khớp với design reference

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Partner Detail, UI |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | UI verification |

**Precondition:**
- Có attachment design reference `image-20260526-034313.png` từ ticket.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở Partner Detail trên Staging | — |
| 2 | So sánh layout, font, spacing, color với design reference | Attachment: `image-20260526-034313.png` |
| 3 | Kiểm tra label text chính xác | — |
| 4 | Kiểm tra format hiển thị số tiền (currency symbol, decimal, separator) | VD: `$1,234.56` |
| 5 | Kiểm tra alignment các elements trong section | — |

**Expected Result:**
- Bước 2: Layout khớp 100% với design reference.
- Bước 3: Label text đúng: "Total Balance", "Link-based Affiliate Balance", "Network Affiliate Balance".
- Bước 4: Số tiền format đúng (dấu phân cách nghìn, 2 decimal, currency symbol).
- Bước 5: Alignment chuẩn, không lệch.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-006 | Verify Balance cập nhật sau khi có commission mới

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, data update |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Partner đang có balance = X.
- Có thể trigger commission mới cho partner.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Ghi nhận balance hiện tại (X) | Screenshot balance |
| 2 | Tạo một transaction/commission mới cho partner | Commission amount = Y |
| 3 | Refresh trang Partner Detail (F5) | — |
| 4 | Kiểm tra giá trị balance mới | — |
| 5 | Verify API response cập nhật đúng | DevTools → Network |

**Expected Result:**
- Bước 4: Balance mới = X + Y (phản ánh commission mới).
- Bước 5: API response chứa balance đã update.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-007 | Verify Balance giảm sau payment thành công

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, payment deduction |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Partner có balance > 0.
- Có quyền thực hiện Commission Payment.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Ghi nhận balance hiện tại (X) | Screenshot |
| 2 | Vào Commission Payment, thực hiện payment cho partner | Payment amount = Z |
| 3 | Quay lại Partner Detail, refresh | — |
| 4 | Kiểm tra balance mới | Expected: X − Z |
| 5 | Verify API response | DevTools → Network |

**Expected Result:**
- Bước 4: Balance mới = X − Z. Giảm đúng số tiền đã payment.
- Bước 5: API response phản ánh đúng balance mới.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-008 | Verify Balance = 0 khi Partner chưa có commission

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, zero state |
| **Jira ref** | EP-5680 |
| **Priority** | P2 — High |
| **Type** | Negative / Boundary |

**Precondition:**
- Partner mới tạo, chưa có commission data.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner Detail của partner chưa có commission | Partner mới |
| 2 | Kiểm tra section Total Balance | — |
| 3 | Kiểm tra không có lỗi JS console | DevTools → Console |

**Expected Result:**
- Bước 2: Section vẫn hiển thị, balance = **$0.00**. Không crash, không hiển thị null/undefined.
- Bước 3: Không có error trong console.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-009 | Verify Balance không hiển thị cho partner type khác

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, scope restriction |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Negative |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Có partner thuộc loại **Referral** hoặc loại khác (không phải Link-based / Network Affiliate).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào Partner Detail của partner loại Referral | Referral partner ID |
| 2 | Kiểm tra xem section Total Balance có hiển thị không | — |
| 3 | Thử partner types khác (Corporate, Merchant, v.v.) | — |

**Expected Result:**
- Bước 2–3: Section Total Balance **KHÔNG hiển thị** cho partner types khác ngoài Link-based & Network Affiliate.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-010 | Verify Balance không bị âm khi payment > commission

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, negative prevention |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Negative / Boundary |

**Precondition:**
- Partner có balance nhỏ (VD: $10).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Ghi nhận balance hiện tại | Balance = $10 |
| 2 | Thử payment vượt quá balance | Amount = $50 |
| 3 | Quay lại Partner Detail, kiểm tra balance | — |

**Expected Result:**
- Balance **không bao giờ hiển thị giá trị âm**.
- Nếu payment bị reject: balance giữ nguyên = $10.
- Nếu payment thành công: balance floor at $0.00 (không âm).

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** Cần confirm với Dev: hệ thống prevent payment khi insufficient balance hay cho phép âm?

---

### TC-EP-PTN-011 | Verify Balance không bao gồm commission types khác

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, scope calculation |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Negative |

**Precondition:**
- Partner có commission từ nhiều loại: Referral, Link-based, Network Affiliate.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Kiểm tra tổng commission từ tất cả loại | Referral: $100, Link-based: $50, Network: $30 |
| 2 | Xem balance hiển thị trên Total Balance section | — |
| 3 | Verify balance chỉ gồm Link-based ($50) và Network ($30) | Expected total: $80, NOT $180 |

**Expected Result:**
- Balance chỉ tính từ **Link-based Affiliate** và **Network Affiliate** commissions.
- Referral và các loại khác **KHÔNG** được tính vào balance.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-012 | Verify Balance với giá trị lớn — format số liệu

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, display format |
| **Jira ref** | EP-5680 |
| **Priority** | P2 — High |
| **Type** | Edge / UI |

**Precondition:**
- Partner có balance lớn (nhiều chữ số).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Kiểm tra balance hiển thị với giá trị nhỏ | $0.01 |
| 2 | Kiểm tra balance hiển thị với giá trị trung bình | $1,234.56 |
| 3 | Kiểm tra balance hiển thị với giá trị lớn | $999,999.99 |
| 4 | Kiểm tra không bị overflow / text bị cắt | — |

**Expected Result:**
- Tất cả: Format đúng dấu phân cách nghìn, 2 decimal places.
- Currency symbol hiển thị nhất quán (SGD / USD).
- Không overflow, không bị cắt text.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-013 | Verify Balance khi nhiều partners — data isolation

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, data isolation |
| **Jira ref** | EP-5680 |
| **Priority** | P2 — High |
| **Type** | Edge / Isolation |
| **API** | `GET /api/partner/v2/{partnerId}` |

**Precondition:**
- Có ít nhất 2 partners: Partner A (balance $100) và Partner B (balance $50).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở Partner Detail của Partner A | Balance expected: $100 |
| 2 | Mở Partner Detail của Partner B (tab mới) | Balance expected: $50 |
| 3 | Thay đổi balance Partner A (thêm commission) | +$20 |
| 4 | Refresh Partner B, kiểm tra balance | — |

**Expected Result:**
- Bước 1–2: Mỗi partner hiển thị balance riêng, đúng giá trị.
- Bước 4: Balance Partner B **không bị ảnh hưởng** bởi thay đổi của Partner A. Vẫn = $50.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-014 | Verify Balance sau khi payment fail — rollback đúng

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, rollback |
| **Jira ref** | EP-5680 |
| **Priority** | P1 — Critical |
| **Type** | Edge / Error handling |

**Precondition:**
- Partner có balance > 0.
- Có cách trigger payment failure.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Ghi nhận balance (X) | Screenshot |
| 2 | Thực hiện payment nhưng trigger fail | VD: invalid payment info |
| 3 | Kiểm tra balance sau payment fail | — |
| 4 | Verify API response | DevTools |

**Expected Result:**
- Bước 3: Balance **rollback** về giá trị ban đầu (X). Không bị trừ.
- Không xảy ra balance mismatch.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-015 | Verify API response time cho Total Balance section

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, performance |
| **Jira ref** | EP-5680 |
| **Priority** | P2 — High |
| **Type** | Non-functional / Performance |

**Precondition:**
- Partner có lượng commission data lớn.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở Partner Detail, observe load time | DevTools → Network → timing |
| 2 | Kiểm tra API response time cho partner detail call | — |
| 3 | Load lại 3 lần, ghi nhận average | — |

**Expected Result:**
- API response time < **3 giây** với data thông thường.
- Section Total Balance render không delay đáng kể so với phần còn lại.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-PTN-016 | Verify không có console error khi load section

| Field | Nội dung |
|-------|---------|
| **Module** | A.6.1 Partnership — Total Balance, stability |
| **Jira ref** | EP-5680 |
| **Priority** | P2 — High |
| **Type** | Non-functional |

**Precondition:**
- Mở DevTools Console trước khi navigate.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở Partner Detail có Total Balance | DevTools Console open |
| 2 | Kiểm tra JS errors trong console | — |
| 3 | Kiểm tra 4xx/5xx requests trong Network tab | — |
| 4 | Thao tác lặp lại (refresh, navigate away, quay lại) 5 lần | — |

**Expected Result:**
- Không có JS error liên quan đến Total Balance.
- Không có 4xx/5xx API calls không mong muốn.
- Không memory leak sau thao tác lặp.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

## Tóm tắt coverage

| AC | Scenario | TC ID | Status |
|----|----------|-------|--------|
| AC1 | UI — Section hiển thị đúng | TC-PTN-001 | |
| AC1 | Link-based Balance chính xác | TC-PTN-002 | |
| AC1 | Network Balance chính xác | TC-PTN-003 | |
| AC1 | Dual type — cả 2 balance | TC-PTN-004 | |
| AC1 | UI khớp design reference | TC-PTN-005 | |
| AC1 | Balance update sau commission | TC-PTN-006 | |
| AC1 | Balance giảm sau payment | TC-PTN-007 | |
| AC1 | Zero balance state | TC-PTN-008 | |
| AC1 | Scope: không hiển thị cho types khác | TC-PTN-009 | |
| AC1 | Negative balance prevention | TC-PTN-010 | |
| AC1 | Scope: chỉ tính đúng commission types | TC-PTN-011 | |
| AC1 | Format số liệu lớn | TC-PTN-012 | |
| AC1 | Data isolation giữa partners | TC-PTN-013 | |
| AC1 | Rollback khi payment fail | TC-PTN-014 | |
| AC1 | Performance — response time | TC-PTN-015 | |
| AC1 | Stability — no console errors | TC-PTN-016 | |

**Tổng: 16 test cases** | Happy path: 7 | Negative: 4 | Edge: 3 | Non-functional: 2

---

## Câu hỏi cần confirm với Dev/BA

1. Section Total Balance có hiển thị cho **tất cả** partner types hay chỉ Link-based & Network Affiliate?
2. Balance tính realtime hay cache? Interval cập nhật?
3. Khi partner vừa là Link-based vừa là Network Affiliate, hiển thị 2 dòng riêng hay gộp 1 tổng?
4. Balance có thể bị âm không? System có prevent payment khi insufficient balance?
5. Currency symbol hiển thị theo setting gì (SGD / USD)?
6. Section Total Balance nằm ở vị trí nào trong Partner Detail (trên hay dưới các section khác)?

---
*Test Cases v1.0 — EP-5680 | Tester: Nhu Huynh*
