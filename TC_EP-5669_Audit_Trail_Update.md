# Test Cases — EP-5669: [Linh][Portal] Update Audit trail for managing user activity admin portal

> **Jira ticket**: [EP-5669](https://eskimo-portal.atlassian.net/browse/EP-5669)
> **Summary**: [Linh][Portal] Update Audit trail for managing user activity admin portal
> **Status hiện tại**: QC | **Sprint**: Current Sprint
> **Priority**: Medium | **Tester**: Nhu Huynh | **Ngày**: 2026-06-02

---

## Tổng quan tính năng

Cập nhật giao diện và tính năng của trang **Audit Trail** trong Admin Portal để quản lý hoạt động của user tốt hơn bằng cách log chi tiết API request/response.

**Phạm vi thay đổi chính:**
- **AC1**: Thêm 3 cột mới (Method, Endpoint, Status), loại bỏ cột Module. Format hiển thị Endpoint (truncate, hover copy), format màu Status (200, 403, 500). Log tất cả API requests kèm request/response data (lưu 1 năm).
- **AC2**: Thêm các filter mới: Search by Endpoint, Method Filter, Status Filter, User Filter.
- **AC3**: Sắp xếp lại thứ tự các cột (Time, Method, Event, Endpoint, Status, User, ID, IP, Detail, Level) và thứ tự các filter.

---

## Danh sách Test Case

| TC ID | Tên | Type | Priority | AC |
|-------|-----|------|----------|----|
| TC-EP-AUDT-001 | Verify UI - Layout, Columns & Filters Order | UI / Happy path | P1 | AC1, AC3 |
| TC-EP-AUDT-002 | Verify Method Column & Data Retention limit | Functional | P1 | AC1 |
| TC-EP-AUDT-003 | Verify Endpoint Column Format & Copy Action | Functional / UI | P1 | AC1 |
| TC-EP-AUDT-004 | Verify Status Column Formatting & Colors | UI | P2 | AC1 |
| TC-EP-AUDT-005 | Verify Search by Endpoint functionality | Functional | P1 | AC2 |
| TC-EP-AUDT-006 | Verify Method Filter functionality | Functional | P1 | AC2 |
| TC-EP-AUDT-007 | Verify Status Filter functionality | Functional | P1 | AC2 |
| TC-EP-AUDT-008 | Verify User Filter functionality | Functional | P1 | AC2 |
| TC-EP-AUDT-009 | Integration - API Requests are logged correctly | Integration | P1 | AC1 |

---

## Chi tiết Test Case

---

### TC-EP-AUDT-001 | Verify UI - Layout, Columns & Filters Order

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P1 — Critical |
| **Type** | UI / Happy path |

**Precondition:**
- Đăng nhập Admin Portal với account có quyền xem Audit Trail.
- Có sẵn dữ liệu audit log trong hệ thống.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Truy cập menu **Audit Trail** | — |
| 2 | Quan sát danh sách các bộ lọc (Filters) từ trái sang phải | — |
| 3 | Quan sát danh sách các cột (Columns) của bảng | — |

**Expected Result:**
- Bước 2: Thứ tự filter hiển thị đúng: **Search > Date Time Filter > Method Filter > Status Filter > User Filter > Level Filter**.
- Bước 3: Thứ tự các cột hiển thị đúng: **Time > Method > Event > Endpoint > Status > User > ID > IP > Detail > Level**.
- Cột **Module** cũ không còn hiển thị trên bảng.

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AUDT-002 | Verify Method Column & Data Retention limit

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P1 — Critical |
| **Type** | Functional |

**Precondition:**
- Có log API calls cho các method: GET, POST, PATCH, DELETE.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Xem một record bất kỳ trên Audit Trail | — |
| 2 | Kiểm tra nội dung hiển thị trong cột **Method** | — |
| 3 | Xem chi tiết record (Request/Response data) đối với log mới (dưới 1 năm) | — |
| 4 | Xem chi tiết record đối với log quá hạn (> 1 năm) | Dữ liệu log > 1 năm |

**Expected Result:**
- Bước 2: Cột **Method** hiển thị chính xác phương thức API (GET, POST, PATCH, hoặc DELETE).
- Bước 3: Request/Response data hiển thị đầy đủ và chính xác.
- Bước 4: Request/Response data không còn tồn tại (bị xóa tự động), nhưng record Audit Trail vẫn hiển thị các thông tin khác bình thường (no retention limit for record itself).

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AUDT-003 | Verify Endpoint Column Format & Copy Action

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P1 — Critical |
| **Type** | Functional / UI |

**Precondition:**
- Có record với Endpoint dài. Ví dụ: `/api/data-plan-sale-transaction/PaymentDetails/330E14EB-C122-47B8-95D7-A98BC2A71746`

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Xem cột **Endpoint** của một log có endpoint dài | VD: ID ở cuối url |
| 2 | Di chuột (hover) vào giá trị endpoint bị cắt ngắn | — |
| 3 | Click vào icon **Copy** xuất hiện khi hover | — |
| 4 | Paste nội dung vừa copy ra trình duyệt hoặc notepad | — |

**Expected Result:**
- Bước 1: Giá trị endpoint hiển thị trên grid bị cắt ngắn (truncated) tại dấu `/` cuối cùng và thêm `....` (VD: `/api/data-plan-sale-transaction/PaymentDetails....`).
- Bước 2: Tooltip hiển thị toàn bộ Full Endpoint. Xuất hiện icon **Copy**.
- Bước 4: Nội dung paste ra là **Full Endpoint** chính xác.

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AUDT-004 | Verify Status Column Formatting & Colors

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P2 — High |
| **Type** | UI |

**Precondition:**
- Tạo sẵn (hoặc tìm kiếm) các log có status code 200, 403, 500.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Quan sát các dòng log có status `200` | — |
| 2 | Quan sát các dòng log có status `403` | — |
| 3 | Quan sát các dòng log có status `500` | — |

**Expected Result:**
- Bước 1: Status 200 hiển thị màu **Xanh lá (Green)**.
- Bước 2: Status 403 hiển thị màu **Đỏ (Red)**.
- Bước 3: Status 500 hiển thị màu **Xám (Gray)**.

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AUDT-005 | Verify Search by Endpoint functionality

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P1 — Critical |
| **Type** | Functional |

**Precondition:**
- Trang Audit Trail đã tải thành công.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Tại ô Search, nhập endpoint đầy đủ hoặc một phần | VD: `/api/data-plan` |
| 2 | Nhấn Enter hoặc nút Search | — |

**Expected Result:**
- Hệ thống gọi API `POST /api/audit-trails/search` và trả về danh sách các log có chứa endpoint được search.
- Kết quả tìm kiếm chính xác, hiển thị dữ liệu khớp với text đã nhập.

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AUDT-006 | Verify Method Filter functionality

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P1 — Critical |
| **Type** | Functional |

**Precondition:**
- Dữ liệu audit bao gồm nhiều method khác nhau.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở dropdown **Method Filter** | — |
| 2 | Kiểm tra các options có sẵn | Expected: All, GET, POST, PATCH, DELETE |
| 3 | Chọn thử từng filter (POST, GET...) | — |

**Expected Result:**
- Bảng dữ liệu load lại hiển thị đúng các record có Method tương ứng với filter đã chọn. Chọn "All" sẽ hiển thị tất cả.

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AUDT-007 | Verify Status Filter functionality

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P1 — Critical |
| **Type** | Functional |

**Precondition:**
- Dữ liệu audit có các status 200, 403, 500.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở dropdown **Status Filter** | — |
| 2 | Kiểm tra các options có sẵn | Expected: All, 200, 403, 500 |
| 3 | Chọn thử status `403` hoặc `500` | — |

**Expected Result:**
- Bảng chỉ hiển thị các log với Status khớp với filter đã chọn.

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AUDT-008 | Verify User Filter functionality

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P1 — Critical |
| **Type** | Functional |

**Precondition:**
- Đã có dữ liệu log cho nhiều roles khác nhau.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở dropdown **User Filter** | — |
| 2 | Kiểm tra danh sách hiển thị | — |
| 3 | Chọn filter theo một role cụ thể | VD: `Admin` hoặc `Partner` |

**Expected Result:**
- Bước 2: Hiển thị đầy đủ các **User roles** hợp lệ trong hệ thống làm tùy chọn lọc (thay vì nhập tay user id).
- Bước 3: Dữ liệu filter chính xác các log được thực hiện bởi các user có role tương ứng.

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AUDT-009 | Integration - API Requests are logged correctly

| Field | Nội dung |
|-------|---------|
| **Module** | Admin Portal — Audit Trail |
| **Jira ref** | EP-5669 |
| **Priority** | P1 — Critical |
| **Type** | Integration |

**Precondition:**
- Đăng nhập Admin Portal, mở tab Network để theo dõi API requests.

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Thực hiện một hành động tạo mới hoặc update bất kỳ trên Admin Portal | VD: Update Partner hoặc Create Gift Code |
| 2 | Xác định API Endpoint và method được gọi ở tab Network | VD: `POST /api/gift-code` |
| 3 | Vào mục Audit Trail và search theo Endpoint vừa thao tác | — |
| 4 | Kiểm tra record log mới nhất | — |

**Expected Result:**
- Tất cả API requests từ user (kể cả GET) đều được ghi nhận (Logged) trong Audit Trail.
- Record mới hiển thị đầy đủ Method, Endpoint, User thực hiện và kèm theo Request/Response body trong chi tiết.

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

## Câu hỏi cần confirm với Dev/BA

1. Các status code khác (vd: 201, 400, 401, 404) hiển thị màu gì? Hay Status Filter chỉ fix cứng 200, 403, 500?
2. Dữ liệu request/response > 1 năm sẽ bị xóa là do script schedule tự động (Cron job) chạy hay bị truncate ngay lúc query? Cần test coverage cho job xóa dữ liệu này không?
3. Cột `Level` và `Event` lấy dữ liệu tương ứng từ đâu (còn ý nghĩa giống lúc trước không khi đã thêm method/endpoint)?

---
*Test Cases v1.0 — EP-5669 | Tester: Nhu Huynh*
