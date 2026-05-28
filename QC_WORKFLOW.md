# QC WORKFLOW — Eskimo Portal

> **Dự án**: Eskimo Portals (EP)
> **Áp dụng cho**: QA/QC team, tester, account manager review
> **Cập nhật**: 2026-05-27

---

## Mục lục

1. [Quy trình nhận & xử lý ticket Jira](#1-quy-trình-nhận--xử-lý-ticket-jira)
2. [Checklist QC theo từng bước](#2-checklist-qc-theo-từng-bước)
3. [Mẫu viết Test Case](#3-mẫu-viết-test-case)
4. [Mẫu Log Bug lên Jira](#4-mẫu-log-bug-lên-jira)
5. [Quy tắc chung & severity matrix](#5-quy-tắc-chung--severity-matrix)

---

## 1. Quy trình nhận & xử lý ticket Jira

### 1.1 Vòng đời của ticket

```
┌─────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  TO DO  │───▶│   DEV    │───▶│ DEV DONE │───▶│ QC PASS  │───▶│ STG QC   │───▶│   DONE   │
│         │    │   (WIP)  │    │ (ready)  │    │ (Dev env)│    │ (Staging)│    │          │
└─────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
                                                     │                │
                                                     ▼                ▼
                                               ┌──────────┐    ┌──────────┐
                                               │  BUG /   │    │  BUG /   │
                                               │  REOPEN  │    │  REOPEN  │
                                               └──────────┘    └──────────┘
```

### 1.2 Mô tả từng trạng thái

| Status | Người phụ trách | Ý nghĩa | Hành động tiếp theo |
|--------|----------------|---------|---------------------|
| **TO DO** | PM / BA | Ticket đã được tạo, chờ dev nhận | Dev kéo vào sprint và bắt đầu làm |
| **IN PROGRESS** | Developer | Dev đang thực hiện | — |
| **DEV DONE** | Developer | Dev đã hoàn thành, deploy lên Dev env | QC nhận và bắt đầu test trên Dev |
| **QC PASS** | QC | QC đã pass trên Dev env | Lead/PM approve để deploy STG |
| **STG QC** | QC | Đang test trên Staging env | QC verify lại toàn bộ trên STG |
| **DONE** | QC / PM | Pass toàn bộ trên STG, sẵn sàng release | Đóng ticket |
| **REOPEN** | QC | Phát hiện bug, trả lại Dev | Dev fix và chuyển lại DEV DONE |

### 1.3 Quy trình nhận ticket (QC side)

#### Bước 1 — Đọc hiểu ticket trước khi test

- [ ] Đọc toàn bộ **Description** và **Acceptance Criteria**
- [ ] Kiểm tra **Attachment** (mockup, design, API doc liên quan)
- [ ] Đọc **linked issues** (nếu ticket phụ thuộc ticket khác)
- [ ] Hỏi lại BA/Dev nếu AC chưa rõ — **KHÔNG** bắt đầu test khi còn mơ hồ
- [ ] Xác nhận **môi trường test**: branch nào, env nào (Dev / STG), data seed nào

#### Bước 2 — Chuẩn bị test data & môi trường

- [ ] Chuẩn bị account test đủ role (admin, partner, customer, guest)
- [ ] Seed data cần thiết (partner, data plan, gift code, v.v.)
- [ ] Ghi lại URL môi trường và version build đang test
- [ ] Reset state nếu cần (clear cache, logout session cũ)

#### Bước 3 — Thực hiện test

- [ ] Thực hiện theo **Test Case** đã chuẩn bị
- [ ] Test **Happy path** trước, sau đó test **Negative / Edge case**
- [ ] Verify API response song song với UI (dùng DevTools / Postman / Network tab)
- [ ] Chụp màn hình / record video khi phát hiện bug
- [ ] Ghi kết quả vào cột **Status** của Test Case

#### Bước 4 — Kết luận

| Kết quả | Hành động |
|---------|-----------|
| Tất cả test case PASS | Chuyển ticket sang **QC PASS** (Dev env) hoặc **DONE** (STG) |
| Có test case FAIL | Log bug lên Jira → REOPEN ticket gốc hoặc tạo bug ticket mới → notify Dev |
| Chặn bởi blocker | Comment trên ticket, tag Dev/PM, giữ nguyên status |

---

## 2. Checklist QC theo từng bước

### 2.1 Checklist — Dev env (DEV DONE → QC PASS)

#### Functional
- [ ] Tất cả Acceptance Criteria đều được cover
- [ ] API trả về đúng HTTP status code (200, 201, 400, 401, 403, 404, 500…)
- [ ] Dữ liệu hiển thị trên UI khớp với response từ API
- [ ] Các action CRUD hoạt động đúng (Create, Read, Update, Delete)
- [ ] Validation message hiển thị đúng khi nhập sai dữ liệu
- [ ] Permission / role được áp dụng đúng (admin thấy gì, partner thấy gì)

#### UI / UX
- [ ] Layout không bị vỡ trên các breakpoint chính (desktop 1440, 1280)
- [ ] Các trạng thái loading, empty state, error state hiển thị đúng
- [ ] Text không bị overflow, truncate đúng chỗ
- [ ] Các button, link, dropdown hoạt động đúng

#### Non-functional
- [ ] Không có lỗi console (JS error, 4xx/5xx không mong muốn)
- [ ] Thời gian response API hợp lý (< 3s với data thông thường)
- [ ] Không bị memory leak khi thao tác lặp lại

### 2.2 Checklist — STG env (STG QC → DONE)

> Ngoài checklist Dev env, bổ sung thêm:

- [ ] Verify với **production-like data** (volume lớn hơn, đa dạng hơn)
- [ ] Kiểm tra **tích hợp end-to-end** (flow từ portal này sang portal khác)
- [ ] Verify **email / notification** thực sự được gửi đến đúng địa chỉ
- [ ] Kiểm tra **webhook** nếu ticket liên quan đến Stripe / PayPal / Twilio / SingTel
- [ ] Smoke test các tính năng **liền kề** để đảm bảo không regression
- [ ] Verify trên **nhiều browser** nếu là web (Chrome, Firefox, Safari)

---

## 3. Mẫu viết Test Case

### 3.1 Cấu trúc Test Case

```
TC-[EPIC]-[MODULE]-[NNN]
  ├── Title        : Mô tả ngắn gọn chức năng đang test
  ├── Precondition : Điều kiện cần thỏa mãn trước khi chạy test
  ├── Test Steps   : Các bước thực hiện
  ├── Expected     : Kết quả mong đợi
  ├── Actual       : Kết quả thực tế (điền sau khi chạy)
  └── Status       : PASS / FAIL / BLOCKED / SKIP
```

### 3.2 Ví dụ Test Case — Đăng nhập Admin Portal

---

#### TC-EP-AUTH-001 | Admin — Đăng nhập thành công

| Field | Nội dung |
|-------|---------|
| **Module** | Authentication — Admin Login |
| **Jira ref** | EP-XXXX |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |

**Precondition:**
- Có tài khoản admin hợp lệ với email và số điện thoại đã đăng ký
- Hệ thống đang hoạt động bình thường (không maintenance)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Truy cập trang đăng nhập Admin Portal | URL: `https://[admin-domain]/login` |
| 2 | Nhập email hợp lệ vào ô "Email" | `admin@test.com` |
| 3 | Click nút **"Send OTP"** | — |
| 4 | Kiểm tra email nhận OTP | Inbox của `admin@test.com` |
| 5 | Nhập OTP nhận được vào ô "OTP" | OTP 6 chữ số |
| 6 | Click nút **"Verify"** | — |

**Expected Result:**
- Bước 3: Hệ thống gọi `POST /api/authentication/login` → response 200, hiển thị màn hình nhập OTP
- Bước 5: OTP nhận được trong vòng 60 giây
- Bước 6: Hệ thống gọi `POST /api/authentication/otp` → response 200 kèm JWT token, redirect về Dashboard

**Actual Result:** *(điền sau khi test)*

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** *(link screenshot / recording)*

---

#### TC-EP-AUTH-002 | Admin — Đăng nhập với OTP sai

| Field | Nội dung |
|-------|---------|
| **Module** | Authentication — Admin Login |
| **Priority** | P2 — High |
| **Type** | Functional / Negative |

**Precondition:** Đã request OTP thành công (qua TC-EP-AUTH-001 bước 1–3)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Nhập OTP sai vào ô "OTP" | `000000` (sai) |
| 2 | Click nút **"Verify"** | — |
| 3 | Nhập lại OTP sai lần 2, lần 3 | `111111`, `222222` |

**Expected Result:**
- Bước 2: API trả về 400/401, hiển thị thông báo lỗi "OTP không hợp lệ"
- Bước 3: Sau N lần sai liên tiếp (theo config), tài khoản bị tạm khóa OTP hoặc hiển thị cảnh báo

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### 3.3 Template Test Case rỗng (copy & dùng)

```markdown
#### TC-EP-[MODULE]-[NNN] | [Tên tính năng]

| Field | Nội dung |
|-------|---------|
| **Module** |  |
| **Jira ref** | EP- |
| **Priority** | P1 / P2 / P3 |
| **Type** | Functional / UI / Performance / Security |

**Precondition:**
-

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 |  |  |
| 2 |  |  |
| 3 |  |  |

**Expected Result:**
-

**Actual Result:**
-

**Status:** `[ ] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**
```

### 3.4 Quy ước đặt ID

| Ký hiệu | Ý nghĩa | Ví dụ |
|---------|---------|-------|
| `EP` | Eskimo Portals project | — |
| `AUTH` | Module Authentication | TC-EP-AUTH-001 |
| `DASH` | Module Dashboard | TC-EP-DASH-001 |
| `TXN` | Module Transaction | TC-EP-TXN-001 |
| `ESIM` | Module eSIM & Inventory | TC-EP-ESIM-001 |
| `MBR` | Module Membership | TC-EP-MBR-001 |
| `PTN` | Module Partnership | TC-EP-PTN-001 |
| `GC` | Module Gift Code | TC-EP-GC-001 |
| `PAY` | Module Payment | TC-EP-PAY-001 |
| `CNT` | Module Content & Config | TC-EP-CNT-001 |
| `SEC` | Module Security & Fraud | TC-EP-SEC-001 |
| `MOB` | Customer Mobile App | TC-EP-MOB-001 |
| `EXT` | External Partner API | TC-EP-EXT-001 |

---

## 4. Mẫu Log Bug lên Jira

### 4.1 Khi nào tạo bug ticket mới vs REOPEN ticket gốc?

| Tình huống | Hành động |
|-----------|-----------|
| Bug xuất hiện trong quá trình test ticket đang QC | **REOPEN** ticket gốc + comment chi tiết |
| Bug phát hiện ở tính năng **khác** (regression) | Tạo **bug ticket mới** liên kết (link) về ticket gây ra |
| Bug trên **STG** mà Dev env đã pass | Tạo **bug ticket mới** với label `found-on-stg` |
| Bug cũ tái xuất (reopened đã close) | Tạo **bug ticket mới** với label `regression` |

### 4.2 Cấu trúc bug ticket Jira

```
Summary   : [BUG] [Module] — Mô tả ngắn lỗi (< 80 ký tự)
Issue type: Bug
Priority  : Blocker / Critical / Major / Minor / Trivial
Labels    : found-on-dev | found-on-stg | regression | ui-bug | api-bug
Component : Admin Portal | Partner Portal | Mobile App | API
```

### 4.3 Template Description bug ticket

> Copy nội dung sau vào phần **Description** khi tạo bug ticket trên Jira.

---

```markdown
## 🐛 Bug Report

**Ticket liên quan:** EP-[số ticket gốc]
**Môi trường phát hiện:** Dev / Staging / Production
**Build / Branch:** [tên branch hoặc version]
**Ngày phát hiện:** YYYY-MM-DD
**Tester:** [tên QC]

---

### 📋 Steps to Reproduce

1. [Bước 1 — cụ thể, có thể follow được]
2. [Bước 2]
3. [Bước 3]
> *(Ghi rõ URL, role đang dùng, data cụ thể)*

**Test data dùng:**
- Account: `[email/role]`
- Data: `[ID, tên, giá trị cụ thể]`

---

### ❌ Actual Result

> *Mô tả điều gì xảy ra thực tế (kèm screenshot / video)*

```
[Paste error message hoặc API response ở đây nếu có]
```

---

### ✅ Expected Result

> *Mô tả điều lẽ ra phải xảy ra theo Acceptance Criteria / design*

---

### 🌍 Environment

| Field | Value |
|-------|-------|
| URL | `https://[domain]/path` |
| Browser | Chrome 124 / Firefox 125 / Safari 17 |
| OS | Windows 10 / macOS 14 / iOS 17 |
| Role | Admin / Partner / Customer |
| API endpoint | `METHOD /api/path` |
| API response code | 200 / 400 / 500 |

---

### 📎 Evidence

- Screenshot: [đính kèm]
- Video: [link Loom / Drive]
- Console log: [paste hoặc đính kèm]
- Network HAR: [đính kèm nếu cần]

---

### 💡 Notes / Hypothesis

> *(Tùy chọn) Suy đoán nguyên nhân, ticket liên quan, workaround tạm thời*
```

---

### 4.4 Ví dụ bug ticket đã điền

```markdown
## 🐛 Bug Report

**Ticket liên quan:** EP-5420 (Thêm tính năng xuất CSV danh sách membership)
**Môi trường phát hiện:** Dev
**Build / Branch:** feature/EP-5420-export-membership
**Ngày phát hiện:** 2026-05-27
**Tester:** Nhu Huynh

---

### 📋 Steps to Reproduce

1. Đăng nhập Admin Portal bằng tài khoản admin (`admin@eskimo.com`)
2. Vào menu **Membership** → **List**
3. Áp filter: Status = Active, Country = Singapore
4. Click nút **Export CSV**
5. Mở file CSV vừa tải về

**Test data dùng:**
- Account: `admin@eskimo.com` (role: Super Admin)
- Filter: 1.250 records, Status = Active

---

### ❌ Actual Result

File CSV tải về thiếu cột `phoneNumber` — chỉ có 8/9 cột so với spec.

```
id,email,status,createdAt,country,dataUsage,plan,referralCode
```

---

### ✅ Expected Result

File CSV phải có đủ 9 cột bao gồm `phoneNumber` theo Acceptance Criteria:

```
id,email,phoneNumber,status,createdAt,country,dataUsage,plan,referralCode
```

---

### 🌍 Environment

| Field | Value |
|-------|-------|
| URL | `https://admin-dev.eskimo.com/membership` |
| Browser | Chrome 124 |
| OS | Windows 10 |
| Role | Super Admin |
| API endpoint | `POST /api/membership/export-csv` |
| API response code | 200 (nhưng data thiếu field) |

---

### 📎 Evidence

- Screenshot: [đính kèm file_missing_column.png]
- Network: response body của `POST /api/membership/export-csv` không có field `phoneNumber`
```

---

## 5. Quy tắc chung & Severity Matrix

### 5.1 Priority / Severity Matrix

| Severity | Định nghĩa | Ví dụ | SLA fix |
|----------|-----------|-------|---------|
| 🔴 **Blocker** | Chặn hoàn toàn luồng chính, không có workaround | Không login được, crash app | Fix trong ngày |
| 🟠 **Critical** | Tính năng chính bị hỏng, data sai, security risk | Export sai data, payment bị lỗi | Fix trong 1–2 ngày |
| 🟡 **Major** | Tính năng bị hỏng một phần, có workaround | Filter không hoạt động, UI vỡ layout | Fix trong sprint |
| 🔵 **Minor** | Lỗi nhỏ, không ảnh hưởng flow chính | Text sai, icon lệch, tooltip thiếu | Backlog / next sprint |
| ⚪ **Trivial** | Cải tiến UX, typo, suggestion | Màu sắc không đúng design system | Nice to have |

### 5.2 Quy tắc viết bug

- **Một bug = một ticket** — không gộp nhiều lỗi vào 1 ticket
- **Steps phải reproducible** — người khác follow steps là reproduce được
- **Luôn kèm evidence** — không chụp màn hình = không có bug
- **Không assign bug cho Dev** khi chưa verify reproduce được
- **Tag đúng component** để route đúng team (FE / BE / Mobile)

### 5.3 Quy tắc pass/fail test case

| Điều kiện | Kết quả |
|-----------|---------|
| Tất cả expected result đều đúng | ✅ PASS |
| Bất kỳ expected result nào sai | ❌ FAIL |
| Không test được do blocker từ ticket khác | 🚧 BLOCKED (ghi rõ lý do) |
| Tính năng chưa deploy / out of scope | ⏭ SKIP |

### 5.4 Communication protocol

```
Bug Blocker/Critical  → Notify ngay trên Slack channel #ep-bugs + tag @dev + @pm
Bug Major             → Comment trên Jira ticket, chuyển REOPEN
Bug Minor/Trivial     → Comment trên Jira, giữ ticket hoặc tạo separate ticket
Câu hỏi về AC        → Comment @BA trên ticket, không assume
```

---

*QC Workflow v1.0 — Eskimo Portals | Maintained by QC Team*
