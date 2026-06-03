# Test Cases — EP-5285: [Lâm][Admin] Commission Payment (Link-based, Network Affiliate)

> **Jira ticket**: [EP-5285](https://eskimo-portal.atlassian.net/browse/EP-5285)
> **Summary**: Update Commission Payment logic & UI for Link-based and Network Affiliate based on Partner Balance.
> **Status hiện tại**: QC | **Sprint**: Current Sprint
> **Priority**: Medium | **Tester**: Nhu Huynh | **Ngày**: 2026-06-02
> **Environment**: Staging
> **BE ticket**: [EA-3776](https://eskimoapi.atlassian.net/browse/EA-3776)

---

## Tổng quan tính năng

- **AC1**: Áp dụng UI mới cho màn hình Commission Payment (chỉ hiển thị 2 loại: Network Affiliate & Link-based Affiliate).
- **AC2**: Cập nhật logic tính Partner Balance. Mỗi partner có balance riêng biệt được tính từ các khoản commission của Network & Link-based Affiliate.
- **AC3**: Execute payment to partner (thực hiện thanh toán dựa trên balance, với trạng thái Upcoming và Insufficient).

---

## APIs liên quan

| # | API | Method | Mục đích |
|---|-----|--------|----------|
| 1 | `/api/commission/payment/list` | GET | Lấy danh sách payment list, kiểm tra filter affiliate types |
| 2 | `/api/partner/balance` | GET | Lấy thông tin balance của partner để hiển thị |
| 3 | `/api/commission/payment/execute` | POST | Thực hiện payment cho các upcoming commissions |
| 4 | `/api/commission/payment/recalculate` | POST | BE recalculate partner balance |

---

## Danh sách Test Case

| TC ID | Tên | Type | Priority | AC |
|-------|-----|------|----------|-----|
| TC-EP-CPAY-001 | Verify UI mới của màn hình Commission Payment hiển thị đúng design | UI | P1 | AC1 |
| TC-EP-CPAY-002 | Verify Commission Payment chỉ hiển thị data của Network Affiliate & Link-based Affiliate | Functional | P1 | AC1 |
| TC-EP-CPAY-003 | Verify Partner Balance được tính và cập nhật độc lập cho từng partner | Functional | P1 | AC2 |
| TC-EP-CPAY-004 | Verify Partner Balance tăng đúng khi commission chuyển sang Approved | Functional | P1 | AC2 |
| TC-EP-CPAY-005 | Verify Partner Balance giảm đúng khi payment execute thành công | Functional | P1 | AC2 |
| TC-EP-CPAY-006 | Verify Partner Balance không bao gồm các loại affiliate khác (Referral, Corporate...) | Negative | P1 | AC2 |
| TC-EP-CPAY-007 | Verify Partner Balance rollback lại giá trị cũ nếu payment failed | Edge/Functional | P1 | AC2 |
| TC-EP-CPAY-008 | Verify tính năng BE recalculate partner balance hoạt động chính xác | Backend | P1 | AC2 |
| TC-EP-CPAY-009 | Verify hiển thị Commission status "Upcoming" và "Insufficient" | UI/Functional | P2 | AC3 |
| TC-EP-CPAY-010 | Verify Execute Payment thành công với các commissions "Upcoming" | Functional | P1 | AC3 |
| TC-EP-CPAY-011 | Verify Execute Payment không thể thực hiện với các commissions "Insufficient" | Negative | P1 | AC3 |
| TC-EP-CPAY-012 | Verify payment action sử dụng amount trừ trực tiếp từ Partner Balance | Functional | P1 | AC3 |
| TC-EP-CPAY-013 | Verify Pay Selected Commissions qua popup UI mới hoạt động chính xác | Functional | P1 | AC3 |
| TC-EP-CPAY-014 | Verify Data isolation giữa nhiều partner khác nhau trong danh sách | Edge | P2 | All |
| TC-EP-CPAY-015 | Verify hiệu năng load danh sách Commission Payment với data lớn | Performance | P2 | AC1 |
| TC-EP-CPAY-016 | Verify không có lỗi JS Console / 500 error khi thao tác thanh toán | Stability | P2 | AC3 |

---

## Chi tiết Test Case

---

### TC-EP-CPAY-001 | Verify UI mới của màn hình Commission Payment hiển thị đúng design

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Transactions — Commission Payment |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | UI / Happy path |

**Precondition:**
- Admin account đã login vào Admin Portal với quyền view Transactions.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Truy cập Transactions → Commission Payment | — |
| 2 | Kiểm tra layout tổng thể, font chữ, màu sắc, khoảng cách | — |
| 3 | So sánh với UI Figma link trong ticket | Figma link AC1 |
| 4 | Kiểm tra các column headers trong table | — |

**Expected Result:**
- Màn hình Commission Payment hiển thị UI mới khớp 100% với Figma.
- Headers, buttons và filter sections đúng với design.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-002 | Verify Commission Payment chỉ hiển thị data của Network Affiliate & Link-based Affiliate

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Transactions — Commission Payment |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `GET /api/commission/payment/list` |

**Precondition:**
- Có data commission payment của nhiều loại (Referral, Network, Link-based, Merchant).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở trang Commission Payment list | — |
| 2 | Kiểm tra danh sách các record hiện có | — |
| 3 | Thử dùng filter Affiliate Type (nếu có) | — |
| 4 | Kiểm tra API response list data | DevTools |

**Expected Result:**
- Danh sách chỉ hiển thị các bản ghi thuộc loại **Network Affiliate** và **Link-based Affiliate**.
- Không có bất kỳ record nào thuộc các loại khác (Referral, v.v.).
- API chỉ trả về data của 2 loại này.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-003 | Verify Partner Balance được tính và cập nhật độc lập cho từng partner

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Balance Logic |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |

**Precondition:**
- Hệ thống có nhiều partner với commission history khác nhau.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Kiểm tra Partner Balance của Partner A | ID: Partner_A |
| 2 | Kiểm tra Partner Balance của Partner B | ID: Partner_B |
| 3 | Tạo commission mới cho Partner A | — |
| 4 | Kiểm tra lại balance của cả 2 partner | — |

**Expected Result:**
- Balance của từng partner tính đúng dựa trên tổng commission earned trừ đi payment.
- Action trên Partner A không làm ảnh hưởng đến balance hiển thị của Partner B (data isolation).

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-004 | Verify Partner Balance tăng đúng khi commission chuyển sang Approved

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Balance Update |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |

**Precondition:**
- Partner đang có Pending/New commission chờ approve.
- Ghi nhận balance hiện tại là X.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Tìm một commission đang Pending của Partner A | Amount = Y |
| 2 | Chuyển status commission sang **Approved** | — |
| 3 | Xem lại Partner Balance của Partner A | — |

**Expected Result:**
- Partner Balance được cộng thêm số tiền commission tương ứng.
- Balance mới = X + Y.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-005 | Verify Partner Balance giảm đúng khi payment execute thành công

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Execution |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |

**Precondition:**
- Partner có balance đủ lớn để payment. Ghi nhận balance là X.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Tại màn hình Commission Payment, chọn payment cho Partner A | Amount = Z |
| 2 | Thực hiện payment thành công | — |
| 3 | Kiểm tra Partner Balance sau action | — |

**Expected Result:**
- Sau khi payment success, Partner Balance trừ đúng amount thanh toán.
- Balance mới = X - Z.

**Actual Result:** 
- The Total Balance is incorrectly displayed as $0 when checking the Partnership Detail page while a payment is "In Progress".

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** [EP-5701] [STG - PARTNERSHIP] Total Balance incorrectly resets to $0 when a payment is In Progress

---

### TC-EP-CPAY-006 | Verify Partner Balance không bao gồm các loại affiliate khác

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Balance scope |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Negative |

**Precondition:**
- Partner A có nhiều khoản commission: Network ($50), Link-based ($30), Referral ($20).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Check tổng commission của Partner A | Total earned: $100 |
| 2 | Check Partner Balance hiển thị trong module Commission Payment | — |
| 3 | So sánh giá trị | — |

**Expected Result:**
- Balance chỉ = $80 ($50 Network + $30 Link-based).
- Hệ thống KHÔNG tính khoản $20 từ Referral vào Partner Balance ở scope này.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-007 | Verify Partner Balance rollback lại giá trị cũ nếu payment failed

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Execution |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Edge / Functional |

**Precondition:**
- Partner có balance > 0 (X).
- Trigger cố ý một payment failure (do timeout API, thông tin bank sai...).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Thực hiện payment nhưng trigger fail | — |
| 2 | Nhận error message payment fail | — |
| 3 | Xem lại Partner Balance | — |

**Expected Result:**
- Hệ thống xử lý lỗi gracefully.
- Partner Balance **được rollback** về giá trị ban đầu (X). Không bị mất tiền.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-008 | Verify tính năng BE recalculate partner balance hoạt động chính xác

| Field | Nội dung |
|-------|---------|
| **Module** | Backend API — Recalculate Balance |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Backend / Functional |
| **API** | `POST /api/commission/payment/recalculate` |

**Precondition:**
- Có account Postman call API hoặc trigger từ backend admin tool.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Gọi API endpoint recalculate balance cho một partner có nhiều commission | Partner ID |
| 2 | Kiểm tra response success | — |
| 3 | Kiểm tra balance của partner này trên DB và UI | — |

**Expected Result:**
- Tính năng recalculate hoạt động thành công, data consistency được bảo đảm bằng cách tổng hợp lại từ toàn bộ transaction ledger của partner.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-009 | Verify hiển thị Commission status "Upcoming" và "Insufficient"

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Display |
| **Jira ref** | EP-5285 |
| **Priority** | P2 — High |
| **Type** | UI / Functional |

**Precondition:**
- Partner có balance = $50. Có các commission payment records cần thanh toán: Record A ($30), Record B ($60).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Vào màn hình Commission Payment | — |
| 2 | Kiểm tra status hiển thị của Record A ($30) | Expected: Upcoming |
| 3 | Kiểm tra status hiển thị của Record B ($60) | Expected: Insufficient |

**Expected Result:**
- Record nào có amount <= available balance hiển thị status **Upcoming**.
- Record nào có amount > available balance hiển thị status **Insufficient**.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-010 | Verify Execute Payment thành công với các commissions "Upcoming"

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Action |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |

**Precondition:**
- Partner có commission record với status Upcoming (tức là balance >= amount).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Chọn record status Upcoming | — |
| 2 | Click Execute Payment | — |
| 3 | Xác nhận thực hiện | — |
| 4 | Kiểm tra status record sau action | — |

**Expected Result:**
- Thanh toán thành công.
- Record đổi status thành Paid (hoặc status tương ứng).
- Balance giảm tương ứng.

**Actual Result:** 
- A Warning popup is displayed (stating the status is invalid) instead of a Confirmation popup when selecting 1 valid record and clicking Pay.

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** [EP-5700] [STG - COMMISSION PAYMENTS] Warning popup incorrectly displays instead of Confirm popup when paying a valid record

---

### TC-EP-CPAY-011 | Verify Execute Payment không thể thực hiện với các commissions "Insufficient"

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Action |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Negative |

**Precondition:**
- Partner có commission record với status Insufficient (balance < amount).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Chọn record có status Insufficient | — |
| 2 | Kiểm tra trạng thái của button "Execute Payment" hoặc các action payment khác | — |
| 3 | Thử thao tác pay bằng API trực tiếp | Postman |

**Expected Result:**
- Trên UI: Button payment bị disabled hoặc ẩn đi đối với record này. Không thể thao tác.
- Trên API: Bắn API pay record insufficient sẽ trả về error HTTP 400 (Balance insufficient).

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-012 | Verify payment action sử dụng amount trừ trực tiếp từ Partner Balance

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Functional |

**Precondition:**
- Commission payment record (dựa trên unpaid commission logic cũ nay chuyển sang trừ thẳng Partner Balance).

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Thực hiện payment cho partner | — |
| 2 | Kiểm tra source tiền bị deduct | — |
| 3 | Xác nhận với DB record | — |

**Expected Result:**
- Tiền thanh toán luôn trừ từ Partner Balance tổng hợp.
- Thay đổi logic này không làm hỏng tính toán payment status của các module cũ.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-013 | Verify Pay Selected Commissions qua popup UI mới hoạt động chính xác

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment — Pay Selected |
| **Jira ref** | EP-5285 |
| **Priority** | P1 — Critical |
| **Type** | Functional |

**Precondition:**
- Partner có nhiều bản ghi Upcoming.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Tick chọn nhiều record Upcoming trên UI | — |
| 2 | Click "Pay Selected" | — |
| 3 | Kiểm tra popup xác nhận hiển thị UI mới | UI AC1 |
| 4 | Confirm thanh toán | — |

**Expected Result:**
- Popup hiển thị đúng nội dung số tiền, số lượng records được chọn.
- Xử lý payment thành công, tổng tiền deduct vào balance = tổng tiền của các records được chọn.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-014 | Verify Data isolation giữa nhiều partner khác nhau trong danh sách

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment |
| **Jira ref** | EP-5285 |
| **Priority** | P2 — High |
| **Type** | Edge |

**Precondition:**
- Có hàng chục partner khác nhau có commission records.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Load danh sách Commission Payment list | — |
| 2 | Kiểm tra cột Partner Balance hiển thị | — |
| 3 | Pay 1 record của Partner A | — |
| 4 | Quan sát data của Partner B | — |

**Expected Result:**
- Balance và Upcoming/Insufficient status đánh giá độc lập theo balance của từng partner.
- Payment Partner A làm giảm balance của A, status các record của A có thể thay đổi từ Upcoming -> Insufficient nếu balance hụt. Các bản ghi của B không bị ảnh hưởng.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-015 | Verify hiệu năng load danh sách Commission Payment với data lớn

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment |
| **Jira ref** | EP-5285 |
| **Priority** | P2 — High |
| **Type** | Performance |

**Precondition:**
- Database có >10,000 commission records của Network/Link-based affiliate.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Truy cập Commission Payment list | — |
| 2 | Bật Network DevTools | — |
| 3 | Thực hiện chuyển trang, filtering | — |

**Expected Result:**
- Load time < 3s, phân trang hoạt động ổn định.
- Việc filter 2 loại Affiliate type không gây chậm query.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-CPAY-016 | Verify không có lỗi JS Console / 500 error khi thao tác thanh toán

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Commission Payment |
| **Jira ref** | EP-5285 |
| **Priority** | P2 — High |
| **Type** | Stability |

**Precondition:**
- Mở F12 DevTools trong quá trình test.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Tương tác đầy đủ với trang (click, filter, select, pay, pagination) | — |
| 2 | Theo dõi Console tab | — |
| 3 | Theo dõi Network tab | — |

**Expected Result:**
- Không có lỗi uncaught JS (undefined, null errors) trên console.
- Tất cả API trả về HTTP 2xx, 4xx (nếu validate input), không có 500 Internal Server Error.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

## Tóm tắt coverage

| AC | Scenario | TC ID | Status |
|----|----------|-------|--------|
| AC1 | UI khớp design reference | TC-EP-CPAY-001 | |
| AC1 | Scope Affiliate Type hiển thị | TC-EP-CPAY-002 | |
| AC2 | Balance calculation độc lập | TC-EP-CPAY-003 | |
| AC2 | Balance update (Approved) | TC-EP-CPAY-004 | |
| AC2 | Balance update (Payment success) | TC-EP-CPAY-005 | FAIL |
| AC2 | Balance scope (Không có Referral) | TC-EP-CPAY-006 | |
| AC2 | Balance rollback (Payment fail) | TC-EP-CPAY-007 | |
| AC2 | BE recalculate feature | TC-EP-CPAY-008 | |
| AC3 | Status Upcoming & Insufficient | TC-EP-CPAY-009 | |
| AC3 | Pay Upcoming commission | TC-EP-CPAY-010 | FAIL |
| AC3 | Prevent pay Insufficient commission | TC-EP-CPAY-011 | |
| AC3 | Payment deduct từ Balance | TC-EP-CPAY-012 | |
| AC3 | Pay Selected popup | TC-EP-CPAY-013 | |
| All | Data isolation | TC-EP-CPAY-014 | |
| All | Performance | TC-EP-CPAY-015 | |
| All | Stability | TC-EP-CPAY-016 | |

**Tổng: 16 test cases** | Happy path/Functional: 9 | Negative: 3 | Edge: 2 | Non-functional: 2

---
*Test Cases v2.0 — EP-5285 | Tester: Nhu Huynh*
