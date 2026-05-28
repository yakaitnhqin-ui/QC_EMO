# Test Cases — EP-5659: ABC AI Salesbot — Sign up & Login via AI

> **Jira ticket**: [EP-5659](https://eskimo-portal.atlassian.net/browse/EP-5659)
> **Summary**: ABC AI Salesbot - Sign up & Login flow via AI (WhatsApp chatbot)
> **Status hiện tại**: Staging QC | **Sprint**: Sprint 95
> **Priority**: High | **Tester**: Nhu Huynh | **Ngày**: 2026-05-27

---

## Tổng quan tính năng

Cho phép user **đăng ký mới** và **đăng nhập** vào Eskimo website thông qua WhatsApp chatbot (ABC Salesbot). Chatbot tạo một **magic link dùng một lần** (hết hạn sau 15 phút), gửi qua WhatsApp, user click link → browser mở modal xử lý theo 6 trường hợp khác nhau.

## APIs liên quan (đối chiếu WBS)

| # | API | Method | Mục đích |
|---|-----|--------|---------|
| 1 | `/api/agents/customer-sign-up/initiate` | POST | Chatbot gọi để tạo magic link (initiate flow) |
| 2 | `/api/agents/customer-sign-up/consume` | POST | Website gọi khi user click magic link để xử lý login/signup |
| 3 | `/api/agents/customer/check-phone` | POST | Kiểm tra số điện thoại đã tồn tại tài khoản chưa |
| 4 | `/api/v2.0/tokens/VerifyOTP-Agent` | POST | Verify token dành riêng cho agent flow |
| 5 | `/api/v2.0/Customer/SignUp` | POST | Tạo tài khoản mới sau khi consume magic link |
| 6 | `/api/v2.0/SMS/whatsapp-webhook` | POST/GET | Webhook nhận sự kiện từ WhatsApp |
| 7 | `/api/Link` | POST | Tạo dynamic link gửi qua WhatsApp |
| 8 | `/api/EmailNotification/SendEmailAfterSignUp` | POST | Gửi email sau khi sign up thành công |

---

## Defects phát hiện trong lần test này

| Bug ID | Severity | Mô tả | Ảnh hưởng |
|--------|----------|-------|----------|
| **BUG-001** | 🔴 Blocker | `POST /api/agents/customer-sign-up/consume` trả về HTTP 500 | 9 TC FAIL, 6 TC BLOCKED |
| **BUG-002** | 🔴 Critical | Nhấn button **OK** trên modal không có phản hồi (silent, không đóng) | TC-003, 007, 011, 019 |

---

## Danh sách Test Case

| TC ID | Tên | Type | Priority | AC | Status |
|-------|-----|------|----------|----|--------|
| TC-EP-AISA-001 | Sign-up intent — chatbot đề nghị tạo tài khoản | Happy path | P1 | AC1 | ✅ PASS |
| TC-EP-AISA-002 | Sign-up — user confirm số điện thoại & nhận magic link | Happy path | P1 | AC1 | ✅ PASS |
| TC-EP-AISA-003 | Sign-up — click magic link (user mới, chưa có session) | Happy path | P1 | AC1 | ❌ FAIL |
| TC-EP-AISA-004 | Sign-up — tài khoản tạo xong, redirect đến Data Balance | Happy path | P1 | AC1 | 🔶 BLOCKED |
| TC-EP-AISA-005 | Sign-up — first/last name & email được phép để trống | Happy path | P2 | AC1 | 🔶 BLOCKED |
| TC-EP-AISA-006 | Login intent — chatbot gợi ý đăng nhập | Happy path | P1 | AC2 | ✅ PASS |
| TC-EP-AISA-007 | Login — click magic link (đã có tài khoản, chưa có session) | Happy path | P1 | AC2 | ❌ FAIL |
| TC-EP-AISA-008 | Login — đăng nhập thành công, redirect đến Home | Happy path | P1 | AC2 | 🔶 BLOCKED |
| TC-EP-AISA-009 | Modal Case 2 — Link đã hết hạn (> 15 phút) | Negative | P1 | AC1/AC2 | ❌ FAIL |
| TC-EP-AISA-010 | Modal Case 3 — Browser đang đăng nhập tài khoản khác | Negative | P1 | AC1/AC2 | ❌ FAIL |
| TC-EP-AISA-011 | Modal Case 4 — Tài khoản bị suspended | Negative | P1 | AC1/AC2 | ❌ FAIL |
| TC-EP-AISA-012 | Modal Case 5 — Link đã dùng rồi (click lần 2) | Negative | P1 | AC1/AC2 | ❌ FAIL |
| TC-EP-AISA-013 | Modal Case 6 — API lỗi khi verify token | Negative | P1 | AC1/AC2 | ❌ FAIL |
| TC-EP-AISA-014 | Edge case — Link expire chính xác tại mốc 15 phút | Edge | P2 | AC1/AC2 | 🔶 BLOCKED |
| TC-EP-AISA-015 | Edge case — User từ chối tạo tài khoản (No/Cancel) | Edge | P2 | AC1 | ✅ PASS |
| TC-EP-AISA-016 | Edge case — Link mở trên Desktop không yêu cầu cài app | Edge | P2 | AC1/AC2 | ❌ FAIL |
| TC-EP-AISA-017 | Edge case — Modal Case 3: OK sign out tài khoản cũ & tiếp tục | Edge | P2 | AC1/AC2 | 🔶 BLOCKED |
| TC-EP-AISA-018 | Edge case — Retry sau khi API fail (Modal Case 6) | Edge | P2 | AC1/AC2 | 🔶 BLOCKED |
| TC-EP-AISA-019 | Edge case — Cancel đóng modal, không tạo session | Edge | P3 | AC1/AC2 | ❌ FAIL |

---

## Chi tiết Test Case

---

### TC-EP-AISA-001 | Sign-up intent — Chatbot đề nghị tạo tài khoản

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents (Mobile) — Sign-up via WhatsApp |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `POST /api/agents/customer-sign-up/initiate` |

**Precondition:**
- User đã có WhatsApp với số điện thoại hợp lệ chưa có tài khoản Eskimo
- Chatbot ABC Salesbot đang hoạt động và được kết nối với Eskimo API
- User đang trong cuộc hội thoại với chatbot

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở WhatsApp, bắt đầu chat với Eskimo Salesbot | Số bot: `[whatsapp-bot-number]` |
| 2 | Nhắn nội dung thể hiện ý định muốn đăng ký / hỏi về Eskimo website | `"I want to create an account"` hoặc `"sign up"` |
| 3 | Quan sát phản hồi của chatbot | — |
| 4 | Chatbot hỏi xác nhận tạo tài khoản → reply **"Yes"** | `"Yes"` |

**Expected Result:**
- Bước 3: Chatbot nhận diện sign-up intent, hiển thị đề nghị: *"Would you like to create an Eskimo account?"*
- Bước 4: Chatbot xác nhận số điện thoại hiện tại đang dùng WhatsApp và hỏi user có muốn dùng số đó để đăng ký không

**Actual Result:** Chatbot nhận diện sign-up intent đúng, hiển thị đề nghị tạo tài khoản. Xác nhận số điện thoại hiện tại qua WhatsApp. API `/initiate` và `/check-phone` chưa được gọi ở bước này.

**Status:** `[x] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-AISA-002 | Sign-up — User confirm số điện thoại & nhận magic link

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Magic link generation |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `POST /api/agents/customer-sign-up/initiate`, `POST /api/agents/customer/check-phone` |

**Precondition:**
- Hoàn thành TC-EP-AISA-001 (chatbot đã hỏi xác nhận số điện thoại)
- Số điện thoại `+6591234567` chưa có tài khoản Eskimo

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Chatbot hiển thị: *"Use +6591234567 to create your account?"* → reply **"Yes"** | `"Yes"` |
| 2 | Quan sát tin nhắn tiếp theo từ chatbot | — |
| 3 | Kiểm tra tin nhắn WhatsApp có chứa magic link | — |
| 4 | Ghi lại thời điểm nhận magic link (để test expiry sau) | Timestamp T₀ |

**Expected Result:**
- Bước 1: Chatbot gọi `POST /api/agents/customer/check-phone` → xác nhận số chưa tồn tại
- Bước 1: Chatbot gọi `POST /api/agents/customer-sign-up/initiate` → tạo magic link thành công
- Bước 2: Chatbot gửi tin nhắn chứa magic link (dạng URL), ví dụ: `https://eskimo.com/auth/magic?token=xxxx`
- Bước 3: Link hợp lệ, chưa expired, dùng được một lần

**Actual Result:** Chatbot gọi `/check-phone` → số chưa tồn tại (đúng). `/initiate` tạo magic link thành công. Magic link gửi qua WhatsApp trong vài giây, format URL hợp lệ. Ghi lại T₀.

**Status:** `[x] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:**

---

### TC-EP-AISA-003 | Sign-up — Click magic link (user mới, browser chưa có session)

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Consume magic link → Modal Case 1 |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `POST /api/agents/customer-sign-up/consume` |

**Precondition:**
- Đã nhận magic link từ TC-EP-AISA-002
- Browser mở không có session Eskimo nào (private window hoặc đã logout)
- Link còn trong vòng 15 phút kể từ T₀

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Click magic link từ WhatsApp → link mở trong browser mặc định | Magic link từ TC-002 |
| 2 | Quan sát modal dialog hiện ra | — |
| 3 | Đọc nội dung modal và kiểm tra số điện thoại hiển thị | Phải là `+6591234567` |
| 4 | Click nút **OK** | — |
| 5 | Quan sát trang sau khi click OK | — |

**Expected Result:**
- Bước 1: Browser mở đúng URL Eskimo website (không yêu cầu cài app)
- Bước 2: Website gọi `POST /api/agents/customer-sign-up/consume` với token từ URL
- Bước 2: Modal hiển thị: *"Continue signing in to Eskimo with +6591234567?"* (đúng Modal Case 1 — Sign up)
- Bước 2: Modal có 2 button: **OK** và **Cancel**
- Bước 4: Tài khoản mới được tạo (`POST /api/v2.0/Customer/SignUp` được gọi)
- Bước 5: Redirect đến trang **Data Balance** (đúng theo AC1: "Go to Data Balance for sign-up cases")

**Actual Result:** Browser mở đúng URL Eskimo. Website gọi `POST /consume` → **HTTP 500**. Modal Case 6 *"Something went wrong. Please try again later."* hiển thị thay vì Modal Case 1. Nhấn **OK** → modal **không đóng**, không có phản hồi, không có API call nào được gửi. **[BUG-001, BUG-002]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** BUG-001 — /consume 500. BUG-002 — OK button silent.

---

### TC-EP-AISA-004 | Sign-up — Tài khoản tạo xong, dữ liệu profile đúng

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Sign-up completion |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `POST /api/v2.0/Customer/SignUp`, `GET /api/v2.0/Customer/Profile` |

**Precondition:**
- Đã hoàn thành TC-EP-AISA-003 (click OK trong modal)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Sau khi redirect đến Data Balance, vào trang **Profile** | — |
| 2 | Kiểm tra số điện thoại trong profile | Phải khớp `+6591234567` |
| 3 | Kiểm tra các field: first name, last name, email | — |
| 4 | Vào Admin Portal → Membership → tìm tài khoản vừa tạo | Admin URL: `/membership` |
| 5 | Verify record trên admin | — |

**Expected Result:**
- Bước 2: Phone number = `+6591234567` đúng
- Bước 3: First name, last name, email **có thể để trống** (không bị lỗi, không forced)
- Bước 4: Record membership tồn tại, status = Active
- Bước 5: Tài khoản được tạo bởi source = `whatsapp_agent` (hoặc tương đương)

**Actual Result:** **BLOCKED** — TC-AISA-003 FAIL do BUG-001 (`/consume` HTTP 500). Sign-up không thực hiện được, không có tài khoản để verify profile. **[BUG-001]**

**Status:** `[ ] PASS  [ ] FAIL  [x] BLOCKED  [ ] SKIP`

**Note / Evidence:** Unblock sau khi BUG-001 được fix và TC-003 PASS.

---

### TC-EP-AISA-005 | Sign-up — First/last name & email được phép để trống

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Profile blank fields |
| **Jira ref** | EP-5659 |
| **Priority** | P2 — High |
| **Type** | Functional / Boundary |
| **API** | `POST /api/v2.0/Customer/SignUp` |

**Precondition:**
- Tài khoản mới được tạo qua magic link (từ TC-EP-AISA-003)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở trang Profile sau khi sign up thành công | — |
| 2 | Xác nhận first name trống | Expected: trống |
| 3 | Xác nhận last name trống | Expected: trống |
| 4 | Xác nhận email trống | Expected: trống |
| 5 | Thử cập nhật first name = `"Test"`, lưu lại | `"Test"` |
| 6 | Reload trang, kiểm tra | — |

**Expected Result:**
- Bước 2–4: Các field này trống, **không hiện validation error**
- Bước 5: Cho phép cập nhật sau khi tạo tài khoản
- Bước 6: Dữ liệu được lưu đúng

**Actual Result:** **BLOCKED** — blocked bởi TC-AISA-003 FAIL. Không có tài khoản mới để test blank fields. **[BUG-001]**

**Status:** `[ ] PASS  [ ] FAIL  [x] BLOCKED  [ ] SKIP`

---

### TC-EP-AISA-006 | Login intent — Chatbot gợi ý đăng nhập (AC2)

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Login via WhatsApp |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `POST /api/agents/customer/check-phone`, `POST /api/agents/customer-sign-up/initiate` |

**Precondition:**
- User đã có tài khoản Eskimo với số `+6598765432`
- User đang chat với chatbot

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Nhắn ý định muốn đăng nhập vào Eskimo | `"I want to log in"` / `"login"` |
| 2 | Chatbot hỏi xác nhận → reply **"Yes"** | `"Yes"` |
| 3 | Chatbot xác nhận số điện thoại → reply **"Yes"** | `"Yes"` |
| 4 | Kiểm tra magic link nhận được | — |

**Expected Result:**
- Bước 1: Chatbot nhận diện login intent
- Bước 2: Chatbot hỏi *"Would you like to log in to your Eskimo account?"*
- Bước 3: `POST /api/agents/customer/check-phone` → phone số tồn tại (existing user)
- Bước 4: Magic link được gửi qua WhatsApp, hợp lệ trong 15 phút

**Actual Result:** Chatbot nhận diện login intent đúng. `/check-phone` xác nhận số tồn tại (existing user). `/initiate` tạo magic link thành công. Link gửi qua WhatsApp, hợp lệ trong 15 phút.

**Status:** `[x] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AISA-007 | Login — Click magic link, redirect đến Home

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Login consume magic link → Modal Case 1 |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |
| **API** | `POST /api/agents/customer-sign-up/consume` |

**Precondition:**
- Đã nhận magic link từ TC-EP-AISA-006
- Browser không có session Eskimo

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Click magic link từ WhatsApp | Magic link từ TC-006 |
| 2 | Quan sát modal dialog | — |
| 3 | Click **OK** | — |
| 4 | Quan sát redirect destination | — |

**Expected Result:**
- Bước 2: Modal Case 1 hiển thị: *"Continue signing in to Eskimo with +6598765432?"*
- Bước 3: `POST /api/agents/customer-sign-up/consume` → login thành công, session được tạo
- Bước 4: Redirect đến **Home** (đúng theo AC1: "Go to Home for login cases")

**Actual Result:** Browser mở đúng URL. `POST /consume` → **HTTP 500**. Modal Case 6 hiển thị thay vì Modal Case 1 (login). Nhấn **OK** → modal **không đóng**, không có phản hồi. Không redirect đến Home. **[BUG-001, BUG-002]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** BUG-001 — /consume 500. BUG-002 — OK button silent.

---

### TC-EP-AISA-008 | Login — Session hợp lệ sau khi đăng nhập

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Login session validation |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Functional / Happy path |

**Precondition:**
- Đã hoàn thành TC-EP-AISA-007 (redirect đến Home thành công)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Kiểm tra header/profile icon ở Home page | — |
| 2 | Vào trang Profile | — |
| 3 | Kiểm tra phone number trong profile | Phải là `+6598765432` |
| 4 | Thực hiện một hành động yêu cầu auth (vd: xem Data Balance) | — |
| 5 | Reload trang → session vẫn tồn tại | — |

**Expected Result:**
- Bước 1: Avatar / tên user hiển thị đúng (đã login)
- Bước 3: Phone number khớp với số đã dùng để login
- Bước 4: Không bị redirect về login page
- Bước 5: Session persistent đúng

**Actual Result:** **BLOCKED** — TC-AISA-007 FAIL. Không có session hợp lệ để verify. **[BUG-001]**

**Status:** `[ ] PASS  [ ] FAIL  [x] BLOCKED  [ ] SKIP`

---

### TC-EP-AISA-009 | Modal Case 2 — Magic link đã hết hạn (> 15 phút)

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Expired magic link |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Negative / Security |
| **API** | `POST /api/agents/customer-sign-up/consume` |

**Precondition:**
- Đã nhận magic link (từ TC-002 hoặc TC-006)
- Chờ quá **15 phút** kể từ thời điểm tạo link (T₀ + 16 phút)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Click magic link đã quá 15 phút | Expired link |
| 2 | Quan sát modal dialog | — |
| 3 | Click **OK** | — |

**Expected Result:**
- Bước 1: `POST /api/agents/customer-sign-up/consume` → API trả về error (token expired)
- Bước 2: Modal Case 2 hiển thị: *"This link is invalid or has expired."*
- Bước 2: Chỉ có button **OK** (không có Cancel, không có Retry)
- Bước 3: Modal đóng lại, user không được login/signup, không có session được tạo

**Actual Result:** Click expired link → `POST /consume` → **HTTP 500** (thay vì 401/400 expired token). Modal Case 6 hiển thị thay vì Modal Case 2. Không thể verify message *"This link is invalid or has expired."* **[BUG-001]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note:** Verify API response khi token expired — nên là 401/400 không phải 500.

---

### TC-EP-AISA-010 | Modal Case 3 — Browser đang login tài khoản khác

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Existing session conflict |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Negative / Edge |
| **API** | `POST /api/agents/customer-sign-up/consume` |

**Precondition:**
- Browser đang có session của tài khoản `other_user@test.com`
- Magic link được tạo cho số `+6591234567` (tài khoản khác)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Trên browser đã login `other_user@test.com`, click magic link | Magic link của `+6591234567` |
| 2 | Quan sát modal dialog | — |
| 3 | **Test path A**: Click **OK** | — |
| 4 | Quan sát kết quả sau khi click OK | — |
| 5 | **Test path B**: Mở tab mới, click link lại → click **Cancel** | — |

**Expected Result:**
- Bước 2: Modal Case 3 hiển thị: *"This browser is currently signed in to another account. Would you like to continue signing in?"*
- Bước 2: Có 2 button: **OK** và **Cancel**
- Bước 3 (Path A): Session của `other_user@test.com` bị sign out, login với `+6591234567` thành công
- Bước 4 (Path A): Redirect đúng (Home nếu login, Data Balance nếu sign up)
- Bước 5 (Path B): Modal đóng, session `other_user@test.com` **vẫn còn nguyên** (không bị sign out)

**Actual Result:** Click link khi browser có session khác → `POST /consume` → **HTTP 500**. Modal Case 6 hiển thị thay vì Modal Case 3. Không verify được conflict resolution behavior. **[BUG-001]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AISA-011 | Modal Case 4 — Tài khoản bị suspended

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Suspended account |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Negative |
| **API** | `POST /api/agents/customer-sign-up/consume`, `POST /api/membership/update-status` |

**Precondition:**
- Có tài khoản `+6597777777` đang bị **suspend** trên Admin Portal
- Magic link được tạo trong vòng 15 phút trước khi account bị suspend, hoặc account đã suspend từ trước

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Admin Portal: suspend tài khoản `+6597777777` | Status → Suspended |
| 2 | Yêu cầu chatbot tạo magic link cho `+6597777777` | — |
| 3 | Click magic link nhận được (còn trong 15 phút) | — |
| 4 | Quan sát modal | — |
| 5 | Click **"Contact Support"** | — |
| 6 | Click **"OK"** trên modal | — |

**Expected Result:**
- Bước 4: Modal Case 4 hiển thị: *"Your account has been suspended. Please contact Eskimo support for assistance."*
- Bước 4: Có 2 button: **Contact Support** và **OK**
- Bước 5: Mở cửa sổ/link support chat (Intercom hoặc tương đương)
- Bước 6: Modal đóng, không tạo session, user không được đăng nhập

**Actual Result:** Click link của account suspended → `POST /consume` → **HTTP 500**. Modal Case 6 hiển thị thay vì Modal Case 4 (*"Your account has been suspended..."*). Nhấn **OK** → modal **không đóng**, không có phản hồi. Contact Support chưa verify được vì không reach được Modal Case 4. **[BUG-001, BUG-002]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** BUG-001 che khuất Modal Case 4. BUG-002 confirm riêng trên Modal Case 6 (OK silent).

---

### TC-EP-AISA-012 | Modal Case 5 — Link đã được dùng rồi (click lần 2)

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — One-time link enforcement |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Negative / Security |
| **API** | `POST /api/agents/customer-sign-up/consume` |

**Precondition:**
- Đã click và sử dụng magic link thành công (login/signup đã xong — TC-003 hoặc TC-007)
- Lưu lại magic link đã dùng

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Logout khỏi tài khoản Eskimo | — |
| 2 | Click lại magic link **đã dùng** lần trước (còn trong 15 phút) | Link đã consumed |
| 3 | Quan sát modal | — |
| 4 | Click **OK** | — |

**Expected Result:**
- Bước 2: `POST /api/agents/customer-sign-up/consume` → API từ chối (token đã used)
- Bước 3: Modal Case 5 hiển thị: *"This link has already been used. Please request a new link from WhatsApp if needed."*
- Bước 3: Chỉ có button **OK**
- Bước 4: Modal đóng, không tạo session, user không được đăng nhập

**Actual Result:** Click used link → `POST /consume` → **HTTP 500** (thay vì error "token already used"). Modal Case 6 hiển thị thay vì Modal Case 5. One-time enforcement không thể verify. **[BUG-001]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note:** Đây là security test quan trọng — đảm bảo link thực sự là **one-time use**.

---

### TC-EP-AISA-013 | Modal Case 6 — API lỗi khi verify token

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — API failure handling |
| **Jira ref** | EP-5659 |
| **Priority** | P1 — Critical |
| **Type** | Negative / Error handling |
| **API** | `POST /api/agents/customer-sign-up/consume` |

**Precondition:**
- Có magic link hợp lệ còn trong hạn
- Cần môi trường simulate API lỗi (hoặc thỏa thuận với Dev để có test endpoint cho 500 error)

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Cắt mạng / đặt môi trường API trả về 500 (thỏa thuận với Dev) | — |
| 2 | Click magic link hợp lệ | Magic link còn hạn |
| 3 | Quan sát modal khi API fail | — |
| 4 | Click **Retry** | — |
| 5 | Restore kết nối API, click Retry lần 2 | — |
| 6 | Click **Cancel** (thay vì Retry) | — |

**Expected Result:**
- Bước 3: Modal Case 6 hiển thị: *"Something went wrong. Please try again later."*
- Bước 3: Có 2 button: **Retry** và **Cancel**
- Bước 4: Re-gọi `POST /api/agents/customer-sign-up/consume` — nếu API vẫn lỗi → modal vẫn hiển thị
- Bước 5: API thành công → xử lý tiếp theo đúng flow (login hoặc signup)
- Bước 6: Modal đóng, không có session được tạo

**Actual Result:** Modal Case 6 hiển thị đúng (vì `/consume` luôn 500 — BUG-001). Retry button click → re-gọi `/consume` → vẫn HTTP 500 → Modal Case 6 vẫn hiển thị (bước 4 PASS). Bước 5 *"Retry thành công sau khi restore API"* **không thực hiện được** vì defect là real bug, không restore được trong môi trường test. **[BUG-001 — partial]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** Bước 3–4 PASS (Modal Case 6 hiển thị đúng, Retry re-calls API đúng). Bước 5–6 không verify được hoàn toàn.

---

### TC-EP-AISA-014 | Edge case — Expiry chính xác tại mốc 15 phút

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Link expiry boundary |
| **Jira ref** | EP-5659 |
| **Priority** | P2 — High |
| **Type** | Edge / Boundary |

**Precondition:**
- Nhận magic link, ghi lại timestamp chính xác T₀

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Click link tại T₀ + 14:30 (30 giây trước expiry) | — |
| 2 | Quan sát → click OK → logout | — |
| 3 | Request magic link mới, ghi T₁ | — |
| 4 | Chờ đến T₁ + 15:00 (đúng lúc hết hạn) → click link | — |
| 5 | Request link mới, chờ T₂ + 15:01 → click link | — |

**Expected Result:**
- Bước 1: Link vẫn còn hợp lệ (trong 15 phút) → flow bình thường
- Bước 4: Boundary case — phụ thuộc implementation (accept hoặc expired)
- Bước 5: Link đã expired → hiển thị Modal Case 2

**Actual Result:** **BLOCKED** — `/consume` luôn trả HTTP 500 bất kể token state (còn hạn hay hết hạn). Không thể phân biệt expired vs valid token để test boundary. **[BUG-001]**

**Status:** `[ ] PASS  [ ] FAIL  [x] BLOCKED  [ ] SKIP`

**Note:** Cần hỏi Dev về giá trị expiry là **< 15min** hay **≤ 15min**.

---

### TC-EP-AISA-015 | Edge case — User từ chối tạo tài khoản

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — User cancels chatbot flow |
| **Jira ref** | EP-5659 |
| **Priority** | P2 — High |
| **Type** | Edge / Negative |

**Precondition:**
- User đang chat với chatbot, chatbot đề nghị tạo tài khoản

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Chatbot hỏi muốn tạo tài khoản không → reply **"No"** | `"No"` |
| 2 | Quan sát phản hồi của chatbot | — |
| 3 | Chatbot hỏi xác nhận số điện thoại → reply **"No"** | `"No"` |

**Expected Result:**
- Bước 1: Chatbot không tạo magic link, không gọi API
- Bước 2: Chatbot có phản hồi phù hợp (ví dụ: *"Okay, let me know if you need anything else."*)
- Bước 3: Flow dừng lại, không có link nào được gửi

**Actual Result:** Chatbot phản hồi đúng khi user trả lời "No". Không tạo magic link, không gọi API backend (`/initiate`, `/check-phone` không được trigger). Flow dừng lại, không có side effect.

**Status:** `[x] PASS  [ ] FAIL  [ ] BLOCKED  [ ] SKIP`

---

### TC-EP-AISA-016 | Edge case — Link mở trên Desktop, không yêu cầu cài app

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Desktop web behavior |
| **Jira ref** | EP-5659 |
| **Priority** | P2 — High |
| **Type** | Edge / Platform |

**Precondition:**
- Có magic link hợp lệ
- Dùng máy tính Desktop (Windows/macOS) với WhatsApp Web hoặc copy link

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Mở magic link trên **Chrome Desktop** | — |
| 2 | Kiểm tra không có popup yêu cầu cài app | — |
| 3 | Thực hiện flow sign-up/login đến khi hoàn thành | — |
| 4 | Mở link trên **Firefox Desktop** | — |
| 5 | Mở link trên **Safari Desktop** (macOS) | — |

**Expected Result:**
- Bước 1–3: Website mở trực tiếp, **không** có banner/popup *"Download the Eskimo app"* hoặc *"Open in app"*
- Bước 2: Modal dialog hiển thị đúng trên desktop browser
- Bước 4–5: Behavior nhất quán trên các browser khác nhau

**Actual Result:** Chrome Desktop: browser mở link thành công, **không có app prompt** ✅. Tuy nhiên `POST /consume` → **HTTP 500** → Modal Case 6 hiển thị, flow không hoàn thành. Cross-browser (Firefox, Safari) chưa verify được vì defect cản toàn bộ flow. **[BUG-001]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

**Note / Evidence:** Bước 2 (no app prompt) PASS riêng. Bước 3–5 FAIL do BUG-001.

---

### TC-EP-AISA-017 | Edge case — Modal Case 3: OK sign out & tiếp tục đúng

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Sign out & proceed |
| **Jira ref** | EP-5659 |
| **Priority** | P2 — High |
| **Type** | Edge / State management |

**Precondition:**
- Browser có session `other_user@test.com`
- Magic link cho `+6591234567`

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Click magic link → Modal Case 3 hiển thị | — |
| 2 | Click **OK** | — |
| 3 | Kiểm tra session của `other_user@test.com` | — |
| 4 | Kiểm tra session của `+6591234567` | — |
| 5 | Mở trang Profile | — |

**Expected Result:**
- Bước 3: Session `other_user@test.com` đã bị **sign out hoàn toàn** (cookie/token cleared)
- Bước 4: Session `+6591234567` được tạo mới và hợp lệ
- Bước 5: Profile hiển thị đúng thông tin của `+6591234567`, không còn data của `other_user`

**Actual Result:** **BLOCKED** — `/consume` trả HTTP 500, không reach được Modal Case 3. Sign-out behavior và session management không thể verify. **[BUG-001]**

**Status:** `[ ] PASS  [ ] FAIL  [x] BLOCKED  [ ] SKIP`

---

### TC-EP-AISA-018 | Edge case — Retry sau API fail thành công

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Retry mechanism |
| **Jira ref** | EP-5659 |
| **Priority** | P2 — High |
| **Type** | Edge / Error recovery |

**Precondition:**
- Đã thấy Modal Case 6 (API fail) từ TC-EP-AISA-013
- API đã được restore

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Modal Case 6 đang hiển thị với button Retry/Cancel | — |
| 2 | Click **Retry** sau khi API đã hoạt động trở lại | — |
| 3 | Quan sát kết quả | — |

**Expected Result:**
- Bước 2: Re-gọi `POST /api/agents/customer-sign-up/consume` với cùng token
- Bước 3: Flow tiếp tục bình thường — hiển thị đúng modal (Case 1 hoặc Case khác tùy trạng thái)
- Token vẫn còn hợp lệ sau lần retry (không bị mark as used do lần fail trước)

**Actual Result:** **BLOCKED** — BUG-001 là real defect, không restore được `/consume` API trong môi trường test hiện tại. Scenario "Retry thành công" không thể thực hiện. **[BUG-001]**

**Status:** `[ ] PASS  [ ] FAIL  [x] BLOCKED  [ ] SKIP`

---

### TC-EP-AISA-019 | Edge case — Cancel modal không tạo side effect

| Field | Nội dung |
|-------|---------|
| **Module** | C.11 AI Agents — Cancel no side effects |
| **Jira ref** | EP-5659 |
| **Priority** | P3 — Medium |
| **Type** | Edge |

**Precondition:**
- Có magic link hợp lệ, browser chưa có session

**Test Steps:**

| # | Step | Test Data |
|---|------|-----------|
| 1 | Click magic link → Modal Case 1 hiển thị | — |
| 2 | Click **Cancel** | — |
| 3 | Kiểm tra session | — |
| 4 | Click lại magic link cùng link trên (còn hạn) | — |
| 5 | Lần này click **OK** | — |

**Expected Result:**
- Bước 2: Modal đóng lại
- Bước 3: Không có session nào được tạo (vẫn là guest)
- Bước 4: Magic link **vẫn còn dùng được** (Cancel không mark link là used)
- Bước 5: Flow sign-up/login diễn ra bình thường

**Actual Result:** Click link → `POST /consume` → **HTTP 500** → Modal Case 6 hiển thị (thay vì Case 1). Cancel click → modal đóng ✅. Kiểm tra session: không có session (✅). Click link lần 2 → `/consume` vẫn HTTP 500. Bước 5 click **OK** → modal **không đóng** (BUG-002). Không verify được "Cancel không mark link là used". **[BUG-001, BUG-002]**

**Status:** `[ ] PASS  [x] FAIL  [ ] BLOCKED  [ ] SKIP`

---

## Tóm tắt coverage theo AC

| AC | Modal Case | TC ID | Status |
|----|-----------|-------|--------|
| AC1 | Sign-up — chatbot detect intent | TC-AISA-001 | ✅ PASS |
| AC1 | Sign-up — confirm phone & generate link | TC-AISA-002 | ✅ PASS |
| AC1 | Case 1 — No session (new user) | TC-AISA-003 | ❌ FAIL |
| AC1 | Sign-up complete — profile & redirect | TC-AISA-004 | 🔶 BLOCKED |
| AC1 | Sign-up — blank profile fields | TC-AISA-005 | 🔶 BLOCKED |
| AC2 | Login — chatbot detect intent | TC-AISA-006 | ✅ PASS |
| AC2 | Case 1 — No session (existing user) | TC-AISA-007 | ❌ FAIL |
| AC2 | Login complete — session & redirect | TC-AISA-008 | 🔶 BLOCKED |
| AC1/AC2 | Case 2 — Link expired | TC-AISA-009 | ❌ FAIL |
| AC1/AC2 | Case 3 — Another account signed in | TC-AISA-010 | ❌ FAIL |
| AC1/AC2 | Case 4 — Account suspended | TC-AISA-011 | ❌ FAIL |
| AC1/AC2 | Case 5 — Link already used | TC-AISA-012 | ❌ FAIL |
| AC1/AC2 | Case 6 — API failure | TC-AISA-013 | ❌ FAIL |
| AC1/AC2 | Edge — 15min boundary | TC-AISA-014 | 🔶 BLOCKED |
| AC1 | Edge — User decline chatbot | TC-AISA-015 | ✅ PASS |
| AC1/AC2 | Edge — Desktop no app prompt | TC-AISA-016 | ❌ FAIL |
| AC1/AC2 | Edge — Case 3 OK sign out correctly | TC-AISA-017 | 🔶 BLOCKED |
| AC1/AC2 | Edge — Retry after API fail | TC-AISA-018 | 🔶 BLOCKED |
| AC1/AC2 | Edge — Cancel no side effect | TC-AISA-019 | ❌ FAIL |

**Tổng: 19 test cases** | ✅ PASS: 4 | ❌ FAIL: 9 | 🔶 BLOCKED: 6

---

## Câu hỏi cần confirm với Dev/BA trước khi test

> ⚠️ Liên hệ confirm trước khi thực hiện test để tránh assumption sai.

1. **API endpoint chính xác** để generate magic link là gì? (`/api/agents/customer-sign-up/initiate` hay khác?)
2. **Token expiry** là strictly `< 15min` hay `≤ 15min`? (ảnh hưởng TC-014)
3. Khi **Retry** trong Modal Case 6 — token có bị revoke sau lần call failed không?
4. **Modal Case 3 (Cancel)** — link có bị mark là used không? (ảnh hưởng TC-019)
5. Trên **mobile browser** (iPhone/Android) — behavior có khác desktop không? Có deep link về app không?
6. Chatbot **ABC Salesbot** đang dùng WhatsApp Business API provider nào (Twilio / Vonage / Meta trực tiếp)?
7. Source của tài khoản được tạo qua flow này lưu là gì trong database?

---

*Test Cases v1.1 — EP-5659 | Eskimo Portals Sprint 95 | Executed: 2026-05-28 | Tester: Nhu Huynh*
*Defects: BUG-001 (Blocker — /consume HTTP 500), BUG-002 (Critical — OK button silent)*
