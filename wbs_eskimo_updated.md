# WBS Eskimo Portal — Cập nhật từ Swagger API

> **Nguồn**: `swagger.json` (Eskimo API v1)
> **Ngày cập nhật**: 2026-05-27
> **Cấu trúc**: Portal → Module → Function → Action (1 dòng = 1 API endpoint)

## Mục lục
- [Quy ước cột](#quy-ước-cột)
- [Tổng quan module](#tổng-quan-module)
- [A. ADMIN WEB PORTAL](#a-admin-web-portal)
  - [A.1 Authentication & Session](#a1-authentication--session)
  - [A.2 Dashboard & Reports](#a2-dashboard--reports)
  - [A.3 Transaction Management](#a3-transaction-management)
  - [A.4 eSim & Inventory Management](#a4-esim--inventory-management)
  - [A.5 Membership & Customer](#a5-membership--customer)
  - [A.6 Partnership Management](#a6-partnership-management)
  - [A.7 Gift Code & Promotional Campaign](#a7-gift-code--promotional-campaign)
  - [A.8 Payment & Withdrawal](#a8-payment--withdrawal)
  - [A.9 Content & Configuration](#a9-content--configuration)
  - [A.10 Security & Fraud](#a10-security--fraud)
- [B. PARTNER WEB PORTAL](#b-partner-web-portal)
- [C. CUSTOMER MOBILE APP](#c-customer-mobile-app)
- [D. EXTERNAL PARTNER API](#d-external-partner-api)
- [E. SYSTEM / WEBHOOK / INTERNAL](#e-system--webhook--internal)

## Quy ước cột

| Cột | Ý nghĩa |
|------|---------|
| # | STT trong module |
| Function | Nhóm chức năng |
| Action | Hành động cụ thể |
| Method | HTTP method |
| Endpoint | API path |
| UI | Estimate UI design (h) |
| Web | Estimate front-end web (h) |
| Mob | Estimate front-end mobile (h) |
| API | Estimate backend API (h) |
| QC | Estimate QA testing (h) |
| Note | Ghi chú đặc biệt |

## Tổng quan module

| # | Portal | Module | Swagger tag | Số endpoint |
|---|--------|--------|-------------|-------------|
| 1 | Admin | Authentication | Authentication, Tokens | 10 |
| 2 | Admin | Dashboard | DashBoard, ManagementReport, AiAnalyticsAgent | ~70 |
| 3 | Admin | Transaction | Transaction, DataPlanSaleTransaction, UserTransaction, Voucher, Invoice, Consignment, Commission, Referral, Cashback, Shopback | ~120 |
| 4 | Admin | eSim & Inventory | Esims, Inventories, DataPlan, NetworkCoverage, Region, CDR, SupportedPhoneModel | ~80 |
| 5 | Admin | Membership | Membership, MemberGroup, Inbox, AccountManagement | ~70 |
| 6 | Admin | Partnership | Partner, Partner Balances, PartnerApiManagement, AssociateAccount, Affiliate | ~80 |
| 7 | Admin | Gift Code & Promo | GiftCode, PromotionalCampaign, CaptivePage | ~50 |
| 8 | Admin | Payment & Withdrawal | Payment, Paypal, TransferWise, WISEPayment, Withdrawal, SettingPayPal | ~40 |
| 9 | Admin | Content & Config | ContentPage, AppConfigImage, HelpContent, Notification, Translate, Master, Currency, Version, Maintenance | ~80 |
| 10 | Admin | Security & Fraud | FraudsAlert, WarningStripe, BlockEmailPattern, Whitelist, Sms, Whatsapp | ~40 |
| 11 | Partner | Partner Portal (subset Admin) | Partner-scoped endpoints | ~50 |
| 12 | Customer | Mobile App | Customer, CustomerV, CardDetailV, UserDataWallet, BHN, Singpass, Jumio, SocialAuth | ~120 |
| 13 | External | Partner API | ExternalAPIAdapter, ExternalApiWorkerManagement | ~25 |
| 14 | System | Webhook & Internal | Hubs, SingTels, Sample, HealthCheck, Webhooks | ~30 |

---

## A. ADMIN WEB PORTAL

### A.1 Authentication & Session

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Admin Login | Send email to get OTP | POST | /api/authentication/login |  |  |  |  |  |
| 2 | Admin Login | Verify OTP & get token | POST | /api/authentication/otp |  |  |  |  |  |
| 3 | Admin Session | Get current user info | GET | /api/v2.0/Customer/MobileUserDetail |  |  |  |  | Reuse from customer |
| 4 | Associate Account | Get associate login profile | GET | /associate-login-profile |  |  |  |  |  |
| 5 | Role & Permission | Get all module permissions | GET | /api/account-permision/all |  |  |  |  |  |
| 6 | Role & Permission | Get role permissions (admin) | GET | /api/account-permision/role-permisions |  |  |  |  |  |
| 7 | Role & Permission | Get role permissions (partnership) | GET | /api/account-permision/partnership/role-permisions |  |  |  |  |  |
| 8 | Role & Permission | Get user permissions | GET | /api/account-permision/user-permisions |  |  |  |  |  |
| 9 | Email Notification | Get email notification config | GET | /api/user/get-email-notification-configurations |  |  |  |  |  |
| 10 | Email Notification | Unsubscribe notification | POST | /api/user/unsubscribe-notification |  |  |  |  |  |
| 11 | Work Invitation | Get work invitation by token | GET | /api/user/work-invitation |  |  |  |  |  |

### A.2 Dashboard & Reports

#### A.2.1 Dashboard cards & charts

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Setting | Get chart settings | GET | /api/dashboard/get-setting-by-chart |  |  |  |  |  |
| 2 | Setting | Save chart settings | POST | /api/dashboard/set-setting-by-chart |  |  |  |  |  |
| 3 | Setting | Save share view setting | POST | /api/dashboard/set-share-view-setting |  |  |  |  |  |
| 4 | Setting | Get share view setting by id | GET | /api/dashboard/get-share-view-setting |  |  |  |  |  |
| 5 | Setting | Get all share view settings | GET | /api/dashboard/get-all-share-view-setting |  |  |  |  |  |
| 6 | Setting | Delete share view setting | DELETE | /api/dashboard/share-view-setting |  |  |  |  |  |
| 7 | Quick check | Check gift code | GET | /api/dashboard/gift-code-check |  |  |  |  |  |
| 8 | Quick check | Check campaign name | GET | /api/dashboard/campaign-name |  |  |  |  |  |
| 9 | Admin members | Member metric card | POST | /api/dashboard/admin-member |  |  |  |  |  |
| 10 | Admin members | Member metric card v2 | POST | /api/dashboard/admin-member-v2 |  |  |  |  |  |
| 11 | Admin data sale | Data sale metric | POST | /api/dashboard/admin-data-sale |  |  |  |  |  |
| 12 | Admin data sale | Data sale filter analytics | GET | /api/dashboard/admin-data-sale/filter-analy |  |  |  |  |  |
| 13 | Admin data sale | Data sale analytics | POST | /api/dashboard/admin-data-sale/analy |  |  |  |  |  |
| 14 | Admin referral | Referral commission card | POST | /api/dashboard/admin-referral-commission |  |  |  |  |  |
| 15 | Admin commission | Commission card | POST | /api/dashboard/admin-commission |  |  |  |  |  |
| 16 | Admin booking | Booking card | POST | /api/dashboard/admin-booking |  |  |  |  |  |
| 17 | Admin conversion | Conversion rate card | POST | /api/dashboard/admin-conversion-rate |  |  |  |  |  |
| 18 | Admin conversion | Conversion rate v2 | POST | /api/dashboard/v2/admin-conversion-rate |  |  |  |  |  |
| 19 | Admin conversion | Create conversion index | POST | /api/dashboard/create-convertion-rate-index |  |  |  |  |  |
| 20 | Save data | Save admin dashboard data | POST | /api/dashboard/save-data-admin-dashboard |  |  |  |  |  |
| 21 | Gift card | Gift card chart | POST | /api/dashboard/gift-card |  |  |  |  |  |
| 22 | Gift card | List gift card | POST | /api/dashboard/list-gift-card |  |  |  |  |  |
| 23 | Data | Data chart | POST | /api/dashboard/data |  |  |  |  |  |
| 24 | Data | Refresh data | POST | /api/dashboard/data/refresh-admin-data |  |  |  |  |  |
| 25 | Data | Country data chart | POST | /api/dashboard/country-data |  |  |  |  |  |
| 26 | Data | Refresh country data | POST | /api/dashboard/country-data/refresh-admin-data |  |  |  |  |  |
| 27 | Data | List data | POST | /api/dashboard/list-data |  |  |  |  |  |
| 28 | Data | Refresh list data | POST | /api/dashboard/list-data/refresh-admin-data |  |  |  |  |  |
| 29 | Data | List country data | POST | /api/dashboard/list-country-data |  |  |  |  |  |
| 30 | Data | Refresh list country data | POST | /api/dashboard/list-country-data/refresh-admin-data |  |  |  |  |  |
| 31 | Data | List country data sale | POST | /api/dashboard/list-country-data-sale |  |  |  |  |  |
| 32 | Data | Refresh list country data sale | POST | /api/dashboard/list-country-data-sale/refresh-admin-data |  |  |  |  |  |
| 33 | Membership | Membership chart | POST | /api/dashboard/membership |  |  |  |  |  |
| 34 | Membership | Membership by country | POST | /api/dashboard/membership/country |  |  |  |  |  |
| 35 | Membership | Refresh membership country (admin) | POST | /api/dashboard/membership/country/refresh-admin-data |  |  |  |  |  |
| 36 | Membership | Refresh membership country (partner) | POST | /api/dashboard/membership/country/refresh-partner-data |  |  |  |  |  |
| 37 | Membership | Membership by gender | POST | /api/dashboard/membership/genders |  |  |  |  |  |
| 38 | Membership | Refresh gender (admin) | POST | /api/dashboard/membership/genders/refresh-admin-data |  |  |  |  |  |
| 39 | Membership | Refresh gender (partner) | POST | /api/dashboard/membership/genders/refresh-partner-data |  |  |  |  |  |
| 40 | Membership | Membership by age | POST | /api/dashboard/membership/age-users |  |  |  |  |  |
| 41 | Membership | Refresh age (admin) | POST | /api/dashboard/membership/age-users/refresh-admin-data |  |  |  |  |  |
| 42 | Membership | Refresh age (partner) | POST | /api/dashboard/membership/age-users/refresh-partner-data |  |  |  |  |  |
| 43 | Membership | Members using eSim | POST | /api/dashboard/membership/use-esims |  |  |  |  |  |
| 44 | Membership | Refresh use-esims (admin) | POST | /api/dashboard/membership/use-esims/refresh-admin-data |  |  |  |  |  |
| 45 | Membership | Refresh use-esims (partner) | POST | /api/dashboard/membership/use-esims/refresh-partner-data |  |  |  |  |  |
| 46 | Membership | List membership | POST | /api/dashboard/list-membership |  |  |  |  |  |
| 47 | Membership | Refresh list membership | POST | /api/dashboard/list-membership/refresh-admin-data |  |  |  |  |  |
| 48 | Partner | Partner card | POST | /api/dashboard/partner |  |  |  |  |  |
| 49 | Export | Export gift card | POST | /api/dashboard/export-gift |  |  |  |  |  |
| 50 | Export | Export voucher | POST | /api/dashboard/export-voucher |  |  |  |  |  |
| 51 | Export | Export member | POST | /api/dashboard/export-member |  |  |  |  |  |
| 52 | Export | Export referral commission | POST | /api/dashboard/export-referral-commission |  |  |  |  |  |
| 53 | Export | Export commission | POST | /api/dashboard/export-commission |  |  |  |  |  |
| 54 | Export | Export member (admin) | POST | /api/dashboard/export-member-admin |  |  |  |  |  |
| 55 | Pie chart | Last update pie chart (set) | POST | /api/dashboard/last-update-data-sale-piechart |  |  |  |  |  |
| 56 | Pie chart | Last update pie chart (get) | GET | /api/dashboard/last-update-data-sale-piechart |  |  |  |  |  |
| 57 | Metrics | Available metric items | GET | /api/dashboard/metric-items-available |  |  |  |  |  |
| 58 | Metrics | Aggregate metrics | POST | /api/dashboard/metrics |  |  |  |  |  |
| 59 | Group by view | Get user settings | GET | /api/dashboard/get-user-settings |  |  |  |  |  |
| 60 | Group by view | Group by view data | POST | /api/dashboard/group-by-view |  |  |  |  |  |
| 61 | Group by view | Upsert group by item | POST | /api/dashboard/upsert-group-by-view-item |  |  |  |  |  |
| 62 | Group by view | Backfill item IDs | POST | /api/dashboard/backfill-group-by-view-item-ids |  |  |  |  |  |

#### A.2.2 Management Report (raw exports)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Overview metric | eSim metric | POST | /api/management-report/esim-metric |  |  |  |  |  |
| 2 | Overview metric | Purchase statistic | POST | /api/management-report/purchase-statistic |  |  |  |  |  |
| 3 | Overview metric | Membership metric | POST | /api/management-report/membership-metric |  |  |  |  |  |
| 4 | Overview metric | Revenue metric | POST | /api/management-report/revenue-metric |  |  |  |  |  |
| 5 | Overview metric | Members metric | POST | /api/management-report/members-metric |  |  |  |  |  |
| 6 | Sale | Date sale transaction | POST | /api/management-report/date-sale-transaction |  |  |  |  |  |
| 7 | Sale | Summary date sale transaction | POST | /api/management-report/summary-date-sale-transaction |  |  |  |  |  |
| 8 | Customer | Customer acquisition | POST | /api/management-report/customer-acquisition |  |  |  |  |  |
| 9 | Referral | Referral metrics | POST | /api/management-report/referral-metrics |  |  |  |  |  |
| 10 | Data usage | Data usage by country | POST | /api/management-report/data-usage-by-country |  |  |  |  |  |
| 11 | Data cost | Import data cost | POST | /api/management-report/import-data-cost-by-country |  |  |  |  |  |
| 12 | Data cost | List data cost | GET | /api/management-report/list-data-cost-by-country |  |  |  |  |  |
| 13 | Data cost | List import history | POST | /api/management-report/list-report-data-cost-by-country |  |  |  |  |  |
| 14 | Data cost | Update data cost | PUT | /api/management-report/update-data-cost-by-country |  |  |  |  |  |
| 15 | Data cost | Delete data cost | DELETE | /api/management-report/delete-data-cost-by-country |  |  |  |  |  |
| 16 | Margin | Overview average margin | POST | /api/management-report/overview-average-margin-metrics |  |  |  |  |  |
| 17 | Margin | Overview data usage costing | POST | /api/management-report/overview-data-usage-costing-metrics |  |  |  |  |  |
| 18 | Export trigger | Trigger export gift code | POST | /api/management-report/trigger-export-gift-code |  |  |  |  |  |
| 19 | Export trigger | Trigger export data overview | POST | /api/management-report/trigger-export-data-overview |  |  |  |  |  |
| 20 | Export trigger | Trigger export data usage | POST | /api/management-report/trigger-export-data-usage |  |  |  |  |  |
| 21 | Export trigger | Trigger export member summary | POST | /api/management-report/trigger-export-data-member-summary |  |  |  |  |  |
| 22 | Export trigger | Trigger export avg margin | POST | /api/management-report/trigger-export-average-margin-metric |  |  |  |  |  |
| 23 | Export trigger | Trigger export raw user | POST | /api/management-report/trigger-export-raw-user |  |  |  |  |  |
| 24 | Export trigger | Trigger export raw referral commission | POST | /api/management-report/trigger-export-raw-referral-commission |  |  |  |  |  |
| 25 | Export trigger | Trigger export raw affiliate commission | POST | /api/management-report/trigger-export-raw-affiliate-commission |  |  |  |  |  |
| 26 | Export trigger | Trigger export raw data sales | POST | /api/management-report/trigger-export-raw-data-sales |  |  |  |  |  |
| 27 | Export trigger | Trigger export raw gift redemption | POST | /api/management-report/trigger-export-raw-gift-redemption |  |  |  |  |  |
| 28 | Export trigger | Trigger export plan data usage | POST | /api/management-report/trigger-export-plan-data-usage |  |  |  |  |  |
| 29 | Export trigger | Trigger export raw eSim CDRs | POST | /api/management-report/trigger-export-raw-esim-cdrs |  |  |  |  |  |

#### A.2.3 Report History & Type

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | History | Search report history | POST | /api/report-history/search |  |  |  |  |  |
| 2 | Type | Search report types | POST | /api/report-type/search |  |  |  |  |  |
| 3 | Type | Create report type | POST | /api/report-type |  |  |  |  |  |
| 4 | Type | Update report type | PUT | /api/report-type |  |  |  |  |  |
| 5 | Type | Get report type detail | GET | /api/report-type/detail/{requestId} |  |  |  |  |  |
| 6 | Type | Delete report type | DELETE | /api/report-type/{requestId} |  |  |  |  |  |

#### A.2.4 AI Analytics Agent (Phase 1)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Chat | Send analytics chat | POST | /api/ai-analytics-agent/chat |  |  |  |  | EP-5667 |
| 2 | Conversation | List conversations | GET | /api/ai-analytics-agent/conversations |  |  |  |  |  |
| 3 | Conversation | Get conversation detail | GET | /api/ai-analytics-agent/conversations/{id} |  |  |  |  |  |
| 4 | Conversation | Delete conversation | DELETE | /api/ai-analytics-agent/conversations/{id} |  |  |  |  |  |

#### A.2.5 KlookReport

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Import | Upload Klook ticket report | POST | /api/v2.0/KlookReport/UploadKlookTicketReport |  |  |  |  |  |
| 2 | Search | Search Klook reports | POST | /api/v2.0/KlookReport/search |  |  |  |  |  |
| 3 | Content | Klook ticket report content | POST | /api/v2.0/KlookReport/ReportContent/KlookTicketReport |  |  |  |  |  |
| 4 | Content | Klook order report content | POST | /api/v2.0/KlookReport/ReportContent/KlookOrderReport |  |  |  |  |  |

### A.3 Transaction Management

#### A.3.1 General Transaction

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | General | Search general transaction | POST | /api/transaction/general/search |  |  |  |  |  |
| 2 | Voucher tx | Search voucher transaction | POST | /api/transaction/voucher/search |  |  |  |  |  |
| 3 | Prepaid tx | Search prepaid (top-up) transaction | POST | /api/transaction/pre-paid/search |  |  |  |  |  |
| 4 | Prepaid tx | Export prepaid transaction | POST | /api/transaction/pre-paid/export |  |  |  |  |  |
| 5 | Top-up import | Validate top-up file | POST | /api/transaction/top-up/validation-import |  |  |  |  |  |
| 6 | Setting | Save default mapping | POST | /api/transaction/set-default-setting |  |  |  |  |  |
| 7 | Setting | Get default mapping | GET | /api/transaction/get-default-setting |  |  |  |  |  |
| 8 | User tx | Search buy-data history | POST | /api/user-transaction/search |  |  |  |  |  |
| 9 | User tx | Auto update price column | POST | /api/user-transaction/auto-update-price-column |  |  |  |  |  |

#### A.3.2 Data Plan Sale Transaction

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | List | Search data plan sale | POST | /api/data-plan-sale-transaction/search |  |  |  |  |  |
| 2 | List | Search by user | POST | /api/data-plan-sale-transaction/search-by-user |  |  |  |  |  |
| 3 | List | Get payment details | GET | /api/data-plan-sale-transaction/PaymentDetails/{Id} |  |  |  |  |  |
| 4 | Export | Export data plan transaction | POST | /api/data-plan-sale-transaction/export |  |  |  |  |  |
| 5 | Fix data | Add missing refund | POST | /api/data-plan-sale-transaction/adding-missing-refund |  |  |  |  |  |
| 6 | Fix data | Update price (POST) | POST | /api/data-plan-sale-transaction/update-price-sale-transaction |  |  |  |  |  |
| 7 | Fix data | Update price (PUT) | PUT | /api/data-plan-sale-transaction/update-price-sale-transaction |  |  |  |  |  |
| 8 | Refund | Refund transaction | POST | /api/data-plan-sale-transaction/Refund |  |  |  |  |  |
| 9 | Refund | Refund via webhook | POST | /api/data-plan-sale-transaction/refund-webhook |  |  |  |  |  |
| 10 | Refund | Set refund webhook | POST | /api/data-plan-sale-transaction/set-refund-webhook |  |  |  |  |  |
| 11 | Refund | Create missing refund | POST | /api/data-plan-sale-transaction/create-missing-refund-transaction |  |  |  |  |  |

#### A.3.3 Voucher & Top-up

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Voucher | Create voucher | POST | /api/voucher |  |  |  |  |  |
| 2 | Voucher | Search voucher | POST | /api/voucher/search |  |  |  |  |  |
| 3 | Voucher | Export voucher | POST | /api/voucher/Export |  |  |  |  |  |
| 4 | Voucher | Update expired date | POST | /api/voucher/update-expired-date |  |  |  |  |  |
| 5 | Voucher | Deactivate voucher | PUT | /api/voucher-deactivate |  |  |  |  |  |
| 6 | Voucher | Refund voucher | POST | /api/voucher/refund |  |  |  |  |  |
| 7 | Voucher | Trigger refund (export) | POST | /api/voucher/trigger-refund |  |  |  |  |  |
| 8 | Voucher | Enable voucher | POST | /api/voucher/enable |  |  |  |  |  |
| 9 | Voucher | Voucher data-purchase list | GET | /api/voucher/data-purchase |  |  |  |  |  |
| 10 | Voucher | Fix old vouchers (Jan 2024) | POST | /api/refund-post-voucher-before-2024-Jannuary-16 |  |  |  |  |  |
| 11 | Top-up | Create top-up | POST | /api/top-up |  |  |  |  |  |
| 12 | Top-up | Update top-up | PUT | /api/top-up |  |  |  |  |  |
| 13 | Top-up | Cancel top-up | PUT | /api/top-up/cancel/{requestId} |  |  |  |  |  |
| 14 | Top-up | Search top-up | POST | /api/top-up/search |  |  |  |  |  |
| 15 | Top-up | Export top-up | POST | /api/top-up/export |  |  |  |  |  |

#### A.3.4 Invoice

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Invoice | Generate invoice | POST | /api/invoice/generate |  |  |  |  |  |
| 2 | Invoice | Create invoice | POST | /api/invoice |  |  |  |  |  |
| 3 | Invoice | Update invoice | POST | /api/invoice/update |  |  |  |  |  |
| 4 | Invoice | Cancel invoice | PUT | /api/invoice/cancel |  |  |  |  |  |
| 5 | Invoice | Search invoices | POST | /api/invoice/search |  |  |  |  |  |
| 6 | Invoice | Export invoice list | POST | /api/invoice/export |  |  |  |  |  |
| 7 | Invoice | Revert invoice | POST | /api/invoice/revert-invoice |  |  |  |  |  |
| 8 | Invoice | Export voucher of invoice | GET | /api/invoice/export-voucher/{invoiceId} |  |  |  |  |  |
| 9 | Invoice | Export upcoming voucher | POST | /api/invoice/export-upcoming-voucher |  |  |  |  |  |
| 10 | Invoice | Postpaid invoice tracking | POST | /api/invoice/postpaid-invoice-tracking |  |  |  |  |  |
| 11 | Invoice issue | View invoice issue | GET | /api/invoice/invoice-issue/{userTransactionNo}/view |  |  |  |  |  |
| 12 | Invoice issue | Update invoice issue | POST | /api/invoice/invoice-issue/{userTransactionNo}/update |  |  |  |  |  |
| 13 | Invoice issue | Re-issue invoice | POST | /api/invoice/invoice-issue/{userTransactionNo}/re-issue |  |  |  |  |  |
| 14 | Invoice issue | Review re-issue | GET | /api/invoice/invoice-issue/{userTransactionNo}/review |  |  |  |  |  |
| 15 | Invoice issue | Export partner invoice | POST | /api/invoice/invoice-issue/export-partner-invoice |  |  |  |  |  |
| 16 | Issue history | Search issue history | POST | /api/invoice-issue-histories/search-invoice-issues |  |  |  |  |  |
| 17 | Issue history | Review issue history | GET | /api/invoice-issue-histories/{invoiceIssueHistoryId}/review |  |  |  |  |  |

#### A.3.5 Consignment Merchant

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Consignment | Create payment | POST | /api/consignment |  |  |  |  |  |
| 2 | Consignment | Data import | POST | /api/consignment/data-import |  |  |  |  |  |
| 3 | Consignment | Import sold voucher | POST | /api/consignment/import-sold-voucher |  |  |  |  |  |
| 4 | Consignment | Search consignment | POST | /api/consignment/search |  |  |  |  |  |
| 5 | Consignment | Export consignment | POST | /api/consignment/export |  |  |  |  |  |
| 6 | Consignment | Pay consignment | POST | /api/consignment/pay |  |  |  |  |  |
| 7 | Consignment | Set default setting v1 | POST | /api/consignment/set-default-setting |  |  |  |  |  |
| 8 | Consignment | Get default setting v1 | GET | /api/consignment/get-default-setting |  |  |  |  |  |
| 9 | Consignment | Set default setting v2 | POST | /api/consignment/set-default-setting-v2 |  |  |  |  |  |
| 10 | Consignment | Get default setting v2 | GET | /api/consignment/get-default-setting-v2 |  |  |  |  |  |
| 11 | Consignment | Notification payment | GET | /api/consignment/notification-payment |  |  |  |  |  |
| 12 | Consignment | Missing report | POST | /api/consignment/missing-report |  |  |  |  |  |
| 13 | Consignment | Import edit consignment | POST | /api/consignment/import-edit-consignment |  |  |  |  |  |
| 14 | Consignment | Confirm import edit | POST | /api/consignment/confirm-import-edit-consignment |  |  |  |  |  |
| 15 | Consignment | Import update voucher booking | POST | /api/consignment/import-update-voucher-booking |  |  |  |  |  |
| 16 | Consignment | Confirm update voucher booking | POST | /api/consignment/confirm-update-voucher-booking |  |  |  |  |  |

#### A.3.6 Commission & Referral

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Commission | Search commission | POST | /api/commission/search |  |  |  |  |  |
| 2 | Commission | Export commission | POST | /api/commission/export |  |  |  |  |  |
| 3 | Commission | Export commission payment | POST | /api/commission/commission-payment/export |  |  |  |  |  |
| 4 | Commission | Search commission payment | POST | /api/commission/search-commission-payment |  |  |  |  |  |
| 5 | Commission | Search custom commission payment | POST | /api/commission/search-commission-custom-payment |  |  |  |  |  |
| 6 | Commission | Get commission payment detail | GET | /api/commission/commission-payment/{CommissionPaymentID} |  |  |  |  |  |
| 7 | Commission | Sync up data | PUT | /api/commission/commission/sync-up-data |  |  |  |  |  |
| 8 | Commission | Pay commission payment | POST | /api/commission/Pay-commission-payment |  |  |  |  |  |
| 9 | Commission | Pay custom commission | POST | /api/commission/Pay-commission-custom-payment |  |  |  |  |  |
| 10 | Commission | Request withdrawal | POST | /api/commission/request-withdrawal |  |  |  |  |  |
| 11 | Commission | Get payment queue | GET | /api/commission/commssionPaymentQueue |  |  |  |  |  |
| 12 | Commission | Get alert | GET | /api/commission/alert |  |  |  |  |  |
| 13 | Referral | Admin commission search | POST | /api/referral-general/referral-commission-admin/search |  |  |  |  |  |
| 14 | Referral | Partner commission search | POST | /api/referral-general/referral-commission-partner/search |  |  |  |  |  |
| 15 | Referral | Admin payment search | POST | /api/referral-general/referral-payment-admin/search |  |  |  |  |  |
| 16 | Referral | Partner payment search | POST | /api/referral-general/referral-payment-partner/search |  |  |  |  |  |
| 17 | Referral | Export admin commission | POST | /api/referral-general/referral-commission-admin/export |  |  |  |  |  |
| 18 | Referral | Export partner commission | POST | /api/referral-general/referral-commission-partner/export |  |  |  |  |  |
| 19 | Referral | Export partner payment | POST | /api/referral-general/referral-payment-partner/export |  |  |  |  |  |
| 20 | Referral | Export admin payment | POST | /api/referral-general/referral-payment-admin/export |  |  |  |  |  |
| 21 | Referral | Pay referral payment | POST | /api/referral-general/pay-referral-payment |  |  |  |  |  |
| 22 | Referral | Get payment detail | GET | /api/referral-general/referral-payment-detail/{referralPaymentID} |  |  |  |  |  |
| 23 | Referral | Get alert | GET | /api/referral-general/alert |  |  |  |  |  |
| 24 | Referral | Get payment queue | GET | /api/referral-general/referralPaymentQueue |  |  |  |  |  |

#### A.3.7 Affiliate Commission (Link-based / Network / Promo Code)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Affiliate | Active partners | GET | /api/v2.0/Affiliate/ActivePartners |  |  |  |  |  |
| 2 | Affiliate | Affiliate root | GET | /api/v2.0/Affiliate |  |  |  |  |  |
| 3 | Affiliate | Referral search | POST | /api/partnership/Affiliate/Referral/search |  |  |  |  |  |
| 4 | Link-based | Click search | POST | /api/link-based-affiliate/click/search |  |  |  |  |  |
| 5 | Link-based | Commission search | POST | /api/link-based-affiliate/commission/search |  |  |  |  |  |
| 6 | Link-based | Click export | POST | /api/link-based-affiliate/click/export |  |  |  |  |  |
| 7 | Link-based | Commission export | POST | /api/link-based-affiliate/commission/export |  |  |  |  |  |
| 8 | Network | Search network commission | POST | /api/network-affiliate-transaction/search |  |  |  |  |  |
| 9 | Network | Update transaction status | POST | /api/network-affiliate-transaction/{transactionId}/status |  |  |  |  |  |
| 10 | Network | Export network commission | POST | /api/network-affiliate-transaction/export |  |  |  |  |  |
| 11 | Promo code | Commission search | POST | /api/promo-code-affiliate/commission/search |  |  |  |  |  |
| 12 | Promo code | Commission export | POST | /api/promo-code-affiliate/commission/export |  |  |  |  |  |

#### A.3.8 Cashback & Shopback Reports

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Cashback | Search cashback | POST | /api/cashback/search |  |  |  |  |  |
| 2 | Cashback | Missing cashback by id | GET | /api/cashback/MissingById/{Id} |  |  |  |  |  |
| 3 | Cashback | Search missing | POST | /api/cashback/cashback-missing/search |  |  |  |  |  |
| 4 | Cashback | Update missing | PUT | /api/cashback/cashback-missing/update |  |  |  |  |  |
| 5 | Cashback | Delete missing | DELETE | /api/cashback/cashback-missing/delete |  |  |  |  |  |
| 6 | Cashback | Export missing | POST | /api/cashback/cashback-missing/export |  |  |  |  |  |
| 7 | Cashback v2 | Process cashback batch | POST | /api/v2.0/CashBack |  |  |  |  |  |
| 8 | Cashback v2 | Correct user balance | POST | /api/v2.0/CashBack/correct-user-balance |  |  |  |  |  |
| 9 | Cashback v2 | Send pending notification | POST | /api/v2.0/CashBack/send-pending-cashback-notification |  |  |  |  |  |
| 10 | Cashback v2 | Report missing (upload) | POST | /api/v2.0/CashBack/Missing |  |  |  |  |  |
| 11 | Cashback Partner | Active partners | GET | /api/v2.0/CashbackPartner/ActivePartners |  |  |  |  |  |
| 12 | Cashback Partner | Search partners | POST | /api/v2.0/CashbackPartner/Partners |  |  |  |  |  |
| 13 | Cashback Partner | Create | POST | /api/v2.0/CashbackPartner/create |  |  |  |  |  |
| 14 | Cashback Partner | Update | PUT | /api/v2.0/CashbackPartner/update |  |  |  |  |  |
| 15 | Cashback Partner | Get partner (query) | GET | /api/v2.0/CashbackPartner |  |  |  |  |  |
| 16 | Cashback Partner | Get partner (path) | GET | /api/v2.0/CashbackPartner/{idPartner} |  |  |  |  |  |
| 17 | Cashback Partner | Delete partner | DELETE | /api/v2.0/CashbackPartner/delete |  |  |  |  |  |
| 18 | Cashback Report | Import | POST | /api/cashback-report/import |  |  |  |  |  |
| 19 | Cashback Report | Validation | POST | /api/cashback-report/validation |  |  |  |  |  |
| 20 | Cashback Report | Set default setting | POST | /api/cashback-report/set-default-setting |  |  |  |  |  |
| 21 | Cashback Report | Get default setting | GET | /api/cashback-report/get-default-setting |  |  |  |  |  |
| 22 | Shopback | Click webhook | POST | /api/shopback |  |  |  |  |  |
| 23 | Shopback | Create order | GET | /api/shopback/create-shopback-order |  |  |  |  |  |
| 24 | Shopback | Validate order | GET | /api/shopback/validate-shopback-order |  |  |  |  |  |
| 25 | Shopback | Search clicks | POST | /api/shopback/Search |  |  |  |  |  |
| 26 | Shopback | Export clicks | POST | /api/shopback/Export |  |  |  |  |  |
| 27 | Shopback | Payment import | POST | /api/shopback/payment/import |  |  |  |  |  |
| 28 | Shopback | Get import setting | GET | /api/shopback/import-setting |  |  |  |  |  |
| 29 | Shopback | Save import setting | POST | /api/shopback/import-setting |  |  |  |  |  |
| 30 | Shopback | Get setting by id | GET | /api/shopback/import-setting/{Id} |  |  |  |  |  |
| 31 | Shopback | Confirm import | POST | /api/shopback/payment/confirm-import |  |  |  |  |  |
| 32 | Shopback | Search payment | POST | /api/shopback/payment/search |  |  |  |  |  |
| 33 | Shopback | Update payment status | POST | /api/shopback/payment/status |  |  |  |  |  |
| 34 | Shopback | Get payment id list | GET | /api/shopback/payment/payment-id-list |  |  |  |  |  |
| 35 | Shopback | Check payment | GET | /api/shopback/payment/check |  |  |  |  |  |
| 36 | Shopback | Import report | POST | /api/shopback/import |  |  |  |  |  |
| 37 | Shopback | Confirm import report | POST | /api/shopback/confirm-import |  |  |  |  |  |
| 38 | Customer click | Search clicks | POST | /api/v2.0/Customer/customer-click/search |  |  |  |  |  |
| 39 | Customer click | Export clicks | POST | /api/v2.0/Customer/customer-click/export |  |  |  |  |  |
| 40 | Customer click | Update click events | PUT | /api/v2.0/Customer/ClickEvents |  |  |  |  |  |
| 41 | Captive page | Search captive records | POST | /api/captive-page-management/Search |  |  |  |  |  |
| 42 | Captive page | Export captive records | POST | /api/captive-page-management/Export |  |  |  |  |  |

### A.4 eSim & Inventory Management

#### A.4.1 eSim operations

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Activate | Activate from mobile | POST | /api/Esims/activate |  |  |  |  |  |
| 2 | Activate | Activate from web | POST | /api/Esims/web-activate |  |  |  |  |  |
| 3 | Activate | Add data plan to eSim | POST | /api/Esims/add-data-plan/{id} |  |  |  |  |  |
| 4 | Manage | Get eSims by userId | GET | /api/Esims/{userId} |  |  |  |  |  |
| 5 | Manage | Transfer data plan | POST | /api/Esims/transfer-data-plan |  |  |  |  |  |
| 6 | Manage | Cancel data plan by id | POST | /api/Esims/cancel-data-plan/{id} |  |  |  |  |  |
| 7 | Manage | Cancel data plan by user | POST | /api/Esims/cancel-data-plan/{userId}/{id} |  |  |  |  |  |
| 8 | Portal action | Activate eSim portal | POST | /api/Esims/activate-esim-portal |  |  |  |  |  |
| 9 | Portal action | Bulk activate eSim portal | POST | /api/Esims/bulk-activate-esim-portal |  |  |  |  |  |
| 10 | Portal action | Add data plan portal | POST | /api/Esims/add-data-plan-portal |  |  |  |  |  |
| 11 | Suspend | Suspend eSim | POST | /api/Esims/suspend-esim |  |  |  |  |  |
| 12 | Status | Change eSim status | PUT | /api/Esims/change-esim-status |  |  |  |  |  |
| 13 | BSS | Search BSS data plans | POST | /api/Esims/search-bss-data-plans |  |  |  |  |  |
| 14 | Status | Get eSim status | GET | /api/Esims/esim-status |  |  |  |  |  |
| 15 | Status | Sync up single | PUT | /api/Esims/sync-up-esim-status |  |  |  |  |  |
| 16 | Status | Sync up list | PUT | /api/Esims/sync-up-list-esim-status |  |  |  |  |  |
| 17 | Singtel | Cancel Singtel data plan | POST | /api/Esims/cancel-singtel-data-plan |  |  |  |  |  |
| 18 | Singtel | Mapping Singtel data | POST | /api/Esims/mapping-singtel-data |  |  |  |  |  |
| 19 | Singtel | Account duplicate check | GET | /api/Esims/account-duplicate-singtel-data |  |  |  |  |  |
| 20 | Notify | Send active code | POST | /api/Esims/send-active-code/{userId} |  |  |  |  |  |
| 21 | Redeem | Revert redeem data | POST | /api/Esims/revert-redeem-data |  |  |  |  |  |
| 22 | Validation | Check activation | POST | /api/Esims/check-activation-data-plan/{id} |  |  |  |  |  |
| 23 | Out of stock | Handle out-of-stock job | POST | /api/Esims/handle-out-of-stock-esim-job |  |  |  |  |  |
| 24 | Status | Validator eSim status | PUT | /api/Esims/validator-esim-status |  |  |  |  |  |
| 25 | Config | Set log query subscriber | POST | /api/Esims/set-configure-lock-query-subscriber |  |  |  |  |  |
| 26 | KYC | Retry update KYC | POST | /api/Esims/re-try-update-kyc |  |  |  |  |  |
| 27 | Subscription | Extend expire date | POST | /api/Esims/extend-subscription-expire-date-by-user-data-plan |  |  |  |  |  |
| 28 | Data | Manual transfer | POST | /api/Data/manual-transfer |  |  |  |  |  |
| 29 | Data | Manual transfer v2 | POST | /api/Data/manual-transfer-v2 |  |  |  |  |  |
| 30 | Data | Reactive transfer data | POST | /api/Data/reactive-transfer-data |  |  |  |  |  |
| 31 | Data | Reactive eSim | POST | /api/Data/reactive-esim |  |  |  |  |  |

#### A.4.2 Inventory

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | List | Search inventory | POST | /api/inventories/search |  |  |  |  |  |
| 2 | Sync | Sync up has-used data plan | POST | /api/inventories/sync-up-has-used-dataplan |  |  |  |  |  |
| 3 | Recycle | Update recycle inventory | PUT | /api/inventories/update-recycle-inventory |  |  |  |  |  |
| 4 | Recycle | Search recycle inventory | POST | /api/inventories/search-recycle-inventory |  |  |  |  |  |
| 5 | Recycle | Search batch recycle | POST | /api/inventories/search-batch-recycle-inventory |  |  |  |  |  |
| 6 | Recycle | Export recycle inventory | POST | /api/inventories/export-recycle-inventory |  |  |  |  |  |
| 7 | File | Search file name | POST | /api/inventories/search-file-name |  |  |  |  |  |
| 8 | File | Search file import | POST | /api/inventories/search-file-import |  |  |  |  |  |
| 9 | File | Get template (deprecated) | GET | /api/inventories/teamplate |  |  |  |  |  |
| 10 | Import | Upload inventory file | POST | /api/inventories/import |  |  |  |  |  |
| 11 | Assign | Assign to user | POST | /api/inventories/assign-to-user |  |  |  |  |  |
| 12 | Manage | Delete inventory | DELETE | /api/inventories |  |  |  |  |  |
| 13 | Manage | Update status | PUT | /api/inventories |  |  |  |  |  |
| 14 | Manage | Update batch status | PUT | /api/inventories/update-batch |  |  |  |  |  |
| 15 | Export | Export inventory | POST | /api/inventories/export |  |  |  |  |  |
| 16 | Provision | Schedule provisioning | POST | /api/inventories/provisioning-esim-schedule |  |  |  |  |  |
| 17 | Provision | Revoke agent assignment | POST | /api/inventories/revoke-agent-esim-assignment/{userId} |  |  |  |  |  |

#### A.4.3 Data Plan Management

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create data plan | POST | /api/data-plan |  |  |  |  |  |
| 2 | CRUD | Update data plan | PUT | /api/data-plan |  |  |  |  |  |
| 3 | CRUD | Get by id | GET | /api/data-plan/{dataPlanId} |  |  |  |  |  |
| 4 | CRUD | Delete data plan | DELETE | /api/data-plan/{dataPlanId} |  |  |  |  |  |
| 5 | Mobile | Mobile data plan by country | GET | /api/data-plan/mobile/{countryId} |  |  |  |  |  |
| 6 | Group | Group details | POST | /api/data-plan/group-details |  |  |  |  |  |
| 7 | Search | Search data plan | POST | /api/data-plan/search |  |  |  |  |  |
| 8 | Search | All data amounts | POST | /api/data-plan/all-data-amounts |  |  |  |  |  |
| 9 | Mobile | Mobile plan list | POST | /api/data-plan/mobile-plan |  |  |  |  |  |
| 10 | Detail | Detail by id/slug | GET | /api/data-plan/details/{dataplanIdOrSlug} |  |  |  |  |  |
| 11 | Dropdown | Portal dropdown list | POST | /api/data-plan/get-portal-dropdown-list |  |  |  |  |  |
| 12 | Dropdown | Corporate dropdown list | POST | /api/data-plan/get-corporate-dropdown-list |  |  |  |  |  |
| 13 | Auto | Auto update amount | POST | /api/data-plan/auto-update-amount-value |  |  |  |  |  |
| 14 | Ribbon | Get ribbon dropdown | GET | /api/data-plan/ribbon-dropdown |  |  |  |  |  |
| 15 | Ribbon | Seed ribbon dropdown | POST | /api/data-plan/ribbon-dropdown-seed |  |  |  |  |  |
| 16 | Corporate | Get purchase discount | POST | /api/data-plan/corporate/get-purchase-discount |  |  |  |  |  |
| 17 | Slug | Create slug | POST | /api/data-plan/create-slug |  |  |  |  |  |
| 18 | Import | Verify from construct file | POST | /api/data-plan/verify-data-from-construct-file |  |  |  |  |  |
| 19 | Flag | Draw flags | GET | /api/data-plan/draw-flags |  |  |  |  |  |
| 20 | Unlimited | Unlimited plans dropdown | GET | /api/data-plan/unlimited-plans-dropdown-list |  |  |  |  |  |
| 21 | Export | Export data plan | POST | /api/data-plan/export |  |  |  |  |  |
| 22 | Import | Import data plan | POST | /api/data-plan/import |  |  |  |  |  |
| 23 | Import | Validation import | POST | /api/data-plan/validation-import |  |  |  |  |  |
| 24 | Update | Update normal/unlimited | GET | /api/data-plan/update-data-for-normal-and-unlimited-dataplan |  |  |  |  |  |
| 25 | Discount | Get discount price | POST | /api/data-plan/get-discount-price-for-dataplans |  |  |  |  |  |

#### A.4.4 User Data Wallet (Admin views)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Wallet | Search wallet v2 (GET) | GET | /api/data-wallet/search/v2 |  |  |  |  |  |
| 2 | Wallet | Search wallet v2 (POST) | POST | /api/data-wallet/search/v2 |  |  |  |  |  |
| 3 | History | Search history v2 | POST | /api/data-wallet/search-history/v2 |  |  |  |  |  |
| 4 | History | Search history v3 | POST | /api/data-wallet/search-history/v3 |  |  |  |  |  |
| 5 | Wallet | Get wallet root | GET | /api/data-wallet |  |  |  |  |  |
| 6 | Wallet | Search by userId v2 | POST | /api/data-wallet/search-userId/v2 |  |  |  |  |  |
| 7 | Replace | Search data replacement | POST | /api/data-wallet/search-data-replacement |  |  |  |  |  |
| 8 | Replace | Replace data plan | POST | /api/data-wallet/replace-data-plan |  |  |  |  |  |
| 9 | Transfer | Search user transfer v2 | POST | /api/data-wallet/search-user-transfer/v2 |  |  |  |  |  |
| 10 | Transfer | Search history transfer | POST | /api/data-wallet/search-history-user-transfer |  |  |  |  |  |
| 11 | Delete | External delete by phone | DELETE | /api/data-wallet/external-delete-data-plan/{phoneNumber} |  |  |  |  |  |
| 12 | Transfer | View history transfer of plan | POST | /api/data-wallet/{userDataPlan}/view-history-transfer |  |  |  |  |  |
| 13 | View | View by subscriptionId | GET | /api/data-wallet/view-subscriptionId/{subscriptionId} |  |  |  |  |  |

#### A.4.5 Network Coverage & Region & Phone Model

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Coverage | Search | POST | /api/network-coverage/search |  |  |  |  |  |
| 2 | Coverage | Create | POST | /api/network-coverage/create |  |  |  |  |  |
| 3 | Coverage | Get all technologies | GET | /api/network-coverage/technology/get-all |  |  |  |  |  |
| 4 | Coverage | Get by id | GET | /api/network-coverage/{networkcoverageId} |  |  |  |  |  |
| 5 | Coverage | Delete | DELETE | /api/network-coverage/{networkcoverageId} |  |  |  |  |  |
| 6 | Coverage | Get all | GET | /api/network-coverage/get-all |  |  |  |  |  |
| 7 | Coverage | Update | PUT | /api/network-coverage |  |  |  |  |  |
| 8 | Coverage | Mobile country list | POST | /api/network-coverage/CountryList-mobile |  |  |  |  |  |
| 9 | Coverage | Seed country & region content | POST | /api/network-coverage/content-seed-for-country-and-region |  |  |  |  |  |
| 10 | Region | Create region | POST | /api/Region/create |  |  |  |  |  |
| 11 | Region | Get all regions | POST | /api/Region/all |  |  |  |  |  |
| 12 | Region | Region blog info | GET | /api/Region/blog-info |  |  |  |  |  |
| 13 | Region | Upload region image | PUT | /api/Region/{id}/image |  |  |  |  |  |
| 14 | Phone | Search supported phones | POST | /api/supported-phone-model/search |  |  |  |  |  |
| 15 | Phone | Create | POST | /api/supported-phone-model |  |  |  |  |  |
| 16 | Phone | Update | PUT | /api/supported-phone-model |  |  |  |  |  |
| 17 | Phone | Detail | GET | /api/supported-phone-model/detail/{requestId} |  |  |  |  |  |
| 18 | Phone | Delete | DELETE | /api/supported-phone-model/{requestId} |  |  |  |  |  |
| 19 | Phone | Check model | GET | /api/supported-phone-model/check/{modelName} |  |  |  |  |  |

#### A.4.6 CDR & Singtel CDR

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CDR | Import history | POST | /api/CDR/import-history |  |  |  |  |  |
| 2 | CDR | eSim data usage | POST | /api/CDR/esim-data-usage |  |  |  |  |  |
| 3 | CDR | Plan type list | GET | /api/CDR/plan-type |  |  |  |  |  |
| 4 | CDR | Import by job | POST | /api/CDR/analytic/import-by-job |  |  |  |  |  |
| 5 | CDR | Analytic search | POST | /api/CDR/analytic/search |  |  |  |  |  |
| 6 | CDR | Usage transaction detail | GET | /api/CDR/esim-data-usage-transaction-detail/{subscriptionId} |  |  |  |  |  |
| 7 | Singtel CDR | Get last datetime log | GET | /api/singtel-call-detail-records/get-last-datetime-log |  |  |  |  |  |
| 8 | Singtel CDR | Get last datetime cdr | GET | /api/singtel-call-detail-records/get-last-datetime-cdr |  |  |  |  |  |
| 9 | Singtel CDR | Import CDR | POST | /api/singtel-call-detail-records/import-CDR |  |  |  |  |  |
| 10 | Singtel CDR | Search cdr | POST | /api/singtel-call-detail-records/search-cdr |  |  |  |  |  |
| 11 | Singtel CDR | Search | POST | /api/singtel-call-detail-records/search |  |  |  |  |  |
| 12 | Singtel CDR | Import | POST | /api/singtel-call-detail-records/import |  |  |  |  |  |

#### A.4.7 Replace Expired Data Plan

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Replace | Retry replace | POST | /api/replace-expired-data/retry |  |  |  |  |  |
| 2 | Replace | Generate report | POST | /api/replace-expired-data/report |  |  |  |  |  |

### A.5 Membership & Customer

#### A.5.1 Membership (list, detail, status)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | List | Search membership | POST | /api/membership/search |  |  |  |  |  |
| 2 | Export | Export (legacy) | POST | /api/membership/export |  |  |  |  |  |
| 3 | Export | Export v2 | POST | /api/membership/export-v2 |  |  |  |  |  |
| 4 | Export | Export CSV | POST | /api/membership/export-csv |  |  |  |  |  |
| 5 | Export | Trigger async export | POST | /api/membership/trigger-export |  |  |  |  |  |
| 6 | Detail | Get by id | GET | /api/membership/{id} |  |  |  |  |  |
| 7 | Detail | Get report detail | GET | /api/membership/Details/Report/{id} |  |  |  |  |  |
| 8 | OTP | Send mail OTP | POST | /api/membership/send-mail-otp |  |  |  |  |  |
| 9 | Purchase | Member detail purchases | POST | /api/membership/Details/DataPurchase |  |  |  |  |  |
| 10 | Cashback | Cashback section search | POST | /api/membership/cashback-section/search |  |  |  |  |  |
| 11 | Referral | Data referral search | POST | /api/membership/data-referral/search |  |  |  |  |  |
| 12 | Status | Update status | POST | /api/membership/update-status |  |  |  |  |  |
| 13 | Status | Trigger update status | POST | /api/membership/trigger-update-status |  |  |  |  |  |
| 14 | UDID | Unblock signup UDID | POST | /api/membership/unblock-signup-UDID |  |  |  |  |  |
| 15 | OTP | Unblock OTP | POST | /api/membership/unblock-otp |  |  |  |  |  |
| 16 | OTP | OTP details by userId | GET | /api/membership/otp-details/{userId} |  |  |  |  |  |
| 17 | Delete | Delete memberships | DELETE | /api/membership |  |  |  |  |  |
| 18 | Inbox | Email & inbox list | POST | /api/membership/email-and-inbox |  |  |  |  |  |
| 19 | Promo | Promo code details | POST | /api/membership/promo-code |  |  |  |  |  |
| 20 | Data | Data usage list | POST | /api/membership/data-usage |  |  |  |  |  |
| 21 | Notif | Membership notification | GET | /api/membership/notification |  |  |  |  |  |
| 22 | Misc | Update platform ecommerce | GET | /api/membership/update-platform-ecommerce |  |  |  |  |  |
| 23 | OTP | Toggle freeze OTP | POST | /api/membership/toggle-freeze-otp |  |  |  |  |  |
| 24 | Blacklist | Search blacklist | POST | /api/membership/black-list/search |  |  |  |  |  |
| 25 | Blacklist | Add blacklist | POST | /api/membership/black-list |  |  |  |  |  |
| 26 | Blacklist | Remove blacklist | DELETE | /api/membership/black-list |  |  |  |  |  |
| 27 | Whitelist | List whitelist | GET | /api/membership/white-list |  |  |  |  |  |
| 28 | Whitelist | Add whitelist | POST | /api/membership/white-list |  |  |  |  |  |
| 29 | Whitelist | Remove whitelist | DELETE | /api/membership/white-list |  |  |  |  |  |
| 30 | Mobile | Update mobile number | PUT | /api/membership/update-mobile-number |  |  |  |  |  |
| 31 | Device | Search device blacklist | POST | /api/membership/device-black-list/search |  |  |  |  |  |
| 32 | Device | Add device blacklist | POST | /api/membership/device-black-list/add |  |  |  |  |  |
| 33 | Device | Remove device blacklist | POST | /api/membership/device-black-list/remove |  |  |  |  |  |
| 34 | Referral | Update user referral code | POST | /api/membership/update-user-referral-code |  |  |  |  |  |
| 35 | Queue | Update user status queue | GET | /api/membership/update-user-status-queue |  |  |  |  |  |
| 36 | Queue | Retry queue | PUT | /api/membership/retry-update-user-status-queue |  |  |  |  |  |
| 37 | Queue | Cancel queue | PUT | /api/membership/cancel-update-user-status-queue |  |  |  |  |  |
| 38 | Queue | Dismiss queue | PUT | /api/membership/dismiss-update-user-status-queue |  |  |  |  |  |
| 39 | eKYC | Jumio search history | POST | /api/membership/jumio-ekyc/search |  |  |  |  |  |
| 40 | eKYC | Reset Jumio limit | POST | /api/membership/jumio-ekyc/reset-limit |  |  |  |  |  |
| 41 | eKYC | Jumio detail | GET | /api/membership/jumio-ekyc/details/{WorkflowExecutionId} |  |  |  |  |  |
| 42 | eKYC | Accept Jumio | GET | /api/membership/jumio-ekyc/details/{WorkflowExecutionId}/accept |  |  |  |  |  |

#### A.5.2 Member Group

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create group | POST | /api/member-group |  |  |  |  |  |
| 2 | CRUD | Update group | PUT | /api/member-group |  |  |  |  |  |
| 3 | Seed | Seed default | POST | /api/member-group/seed |  |  |  |  |  |
| 4 | Filter | Get filter constants | GET | /api/member-group/GroupFilterConstant |  |  |  |  |  |
| 5 | Detail | Get by id | GET | /api/member-group/{Id} |  |  |  |  |  |
| 6 | Detail | Delete by id | DELETE | /api/member-group/{Id} |  |  |  |  |  |
| 7 | List | List users in group | GET | /api/member-group/list-user/{Id} |  |  |  |  |  |
| 8 | List | Search group | POST | /api/member-group/Search |  |  |  |  |  |
| 9 | Count | Count user | POST | /api/member-group/count-user |  |  |  |  |  |
| 10 | Export | Export user | POST | /api/member-group/export-user |  |  |  |  |  |
| 11 | Trigger | Manual trigger | POST | /api/member-group/trigger |  |  |  |  |  |
| 12 | Dropdown | Nationality | GET | /api/member-group/dropdown/nationality |  |  |  |  |  |
| 13 | Dropdown | Related partner | GET | /api/member-group/dropdown/related-partner |  |  |  |  |  |
| 14 | Dropdown | Plan type by type | GET | /api/member-group/dropdown/plan-type/{type} |  |  |  |  |  |
| 15 | Dropdown | Plan type all | GET | /api/member-group/dropdown/plan-type |  |  |  |  |  |
| 16 | Import | Import file | POST | /api/member-group/import-member-group-file |  |  |  |  |  |
| 17 | Setting | Set default setting | POST | /api/member-group/set-default-setting |  |  |  |  |  |
| 18 | Setting | Get default setting | GET | /api/member-group/get-default-setting |  |  |  |  |  |

#### A.5.3 Admin Staff (Account Management)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create admin staff | POST | /api/account-management/Create |  |  |  |  |  |
| 2 | CRUD | Update admin staff | PUT | /api/account-management/Update |  |  |  |  |  |
| 3 | CRUD | Delete admin staff | DELETE | /api/account-management/{iDAccount} |  |  |  |  |  |
| 4 | List | Search admin staff | POST | /api/account-management/search |  |  |  |  |  |
| 5 | List | Get all | GET | /api/account-management |  |  |  |  |  |
| 6 | List | Get detail | GET | /api/account-management/{accountId} |  |  |  |  |  |
| 7 | Export | Export | POST | /api/account-management/export |  |  |  |  |  |
| 8 | Associate | Create associate account | POST | /api/account-management/associate-account |  |  |  |  |  |
| 9 | Associate | Update associate account | PUT | /api/account-management/associate-account |  |  |  |  |  |
| 10 | Associate | Delete associate accounts | DELETE | /api/account-management/associate-account |  |  |  |  |  |
| 11 | Associate | Get associate by id | GET | /api/account-management/associate-account/{id} |  |  |  |  |  |
| 12 | Associate | Search associate accounts | POST | /api/account-management/associate-account/search |  |  |  |  |  |

#### A.5.4 Audit Trails

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Filter | Get filter level | GET | /api/audit-trails/get-filter-level |  |  |  |  |  |
| 2 | List | Search audit | POST | /api/audit-trails/search |  |  |  |  |  |
| 3 | Export | Export audit | POST | /api/audit-trails/export |  |  |  |  |  |

### A.6 Partnership Management

#### A.6.1 Partner CRUD & Search

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Dropdown | Get network | GET | /api/partner/network |  |  |  |  |  |
| 2 | Dropdown | Get source-lead | GET | /api/partner/source-lead |  |  |  |  |  |
| 3 | Dashboard | Partner dashboard | POST | /api/partner/dashboard |  |  |  |  |  |
| 4 | Dashboard | Available metric items | GET | /api/partner/metric-items-available |  |  |  |  |  |
| 5 | CRUD | Create partner v1 | POST | /api/partner |  |  |  |  |  |
| 6 | CRUD | Update partner v1 | PUT | /api/partner |  |  |  |  |  |
| 7 | CRUD | Create partner v2 | POST | /api/partner/v2 |  |  |  |  |  |
| 8 | CRUD | Update partner v2 | PUT | /api/partner/v2 |  |  |  |  |  |
| 9 | List | Search partner | POST | /api/partner/search |  |  |  |  |  |
| 10 | List | Active partner list | GET | /api/partner/active-partner |  |  |  |  |  |
| 11 | List | Partner creator profiles | GET | /api/partner/partner-creator-profiles |  |  |  |  |  |
| 12 | Search | Search referral code | GET | /api/partner/search-referral-code |  |  |  |  |  |
| 13 | Delete | Delete partner | DELETE | /api/partner/{partnershipId} |  |  |  |  |  |
| 14 | Detail | Get partner v1 | GET | /api/partner/{partnerId} |  |  |  |  |  |
| 15 | Detail | Get partner v2 | GET | /api/partner/v2/{partnerId} |  |  |  |  |  |
| 16 | Detail | Get partner credit | GET | /api/partner/credit/{partnerId} |  |  |  |  |  |
| 17 | API key | Generate API key | POST | /api/partner/generate-api-key |  |  |  |  |  |
| 18 | API key | Publish/unpublish key | PUT | /api/partner/publish-key/{status} |  |  |  |  |  |
| 19 | Tooltip | Get tooltip by id | GET | /api/partner/tooltip/{id} |  |  |  |  |  |
| 20 | API log | Search API request | POST | /api/partner/search-api-request |  |  |  |  |  |
| 21 | Sale | Sale support | POST | /api/partner/sale-support |  |  |  |  |  |
| 22 | FX | Search FX configures | POST | /api/partner/search-partnership-exchange-rate-configures |  |  |  |  |  |
| 23 | FX | Create FX config | POST | /api/partner/create-partnership-exchange-rate-configure |  |  |  |  |  |
| 24 | FX | Update FX config | PUT | /api/partner/update-partnership-exchange-rate-configure |  |  |  |  |  |

#### A.6.2 Worker Account (Corporate)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create worker | POST | /api/partner/worker-management |  |  |  |  |  |
| 2 | CRUD | Update worker | PUT | /api/partner/worker-management |  |  |  |  |  |
| 3 | Top-up | Get self top-up | GET | /api/partner/worker-management/self-topup/{userId} |  |  |  |  |  |
| 4 | Top-up | Update self top-up | PUT | /api/partner/worker-management/self-topup |  |  |  |  |  |
| 5 | Top-up | Create all self top-up | POST | /api/partner/worker-management/create-all-self-topup |  |  |  |  |  |
| 6 | List | Search worker | POST | /api/partner/worker-management/search |  |  |  |  |  |
| 7 | List | Trigger update remaining data | POST | /api/partner/worker-management/trigger-update-remaining-data |  |  |  |  |  |
| 8 | List | Queue history | GET | /api/partner/worker-management/queue-history |  |  |  |  |  |
| 9 | Export | Export workers | POST | /api/partner/worker-management/export |  |  |  |  |  |
| 10 | Email | Email signature | GET | /api/partner/worker-management/email-signature |  |  |  |  |  |
| 11 | Top-up | Bulk top-up | POST | /api/partner/worker-management/bulk-top-up |  |  |  |  |  |
| 12 | Profile | User profile | GET | /api/partner/worker-management/users/{userId}/profile |  |  |  |  |  |
| 13 | Profile | eSim profile | GET | /api/partner/worker-management/users/{userId}/esim-profile |  |  |  |  |  |
| 14 | Status | Update status | POST | /api/partner/worker-management/update-status |  |  |  |  |  |
| 15 | Status | Trigger update status | POST | /api/partner/worker-management/trigger-update-status |  |  |  |  |  |
| 16 | Import | Validate import | POST | /api/partner/worker-management/validation-import |  |  |  |  |  |
| 17 | Import | Confirm import | POST | /api/partner/worker-management/confirm-import |  |  |  |  |  |
| 18 | Setting | Set default setting | POST | /api/partner/set-default-setting |  |  |  |  |  |
| 19 | Setting | Get default setting | GET | /api/partner/get-default-setting |  |  |  |  |  |
| 20 | Sync | Sync self top-up | POST | /api/partner/worker-account/sync-self-top-up |  |  |  |  |  |

#### A.6.3 Corporate Payment History & Top-up

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | History | Search payment history | POST | /api/partner/corporate/search-payment-history |  |  |  |  |  |
| 2 | History | Export payment history | POST | /api/partner/corporate/payment-history/export |  |  |  |  |  |
| 3 | Payment | Setup intent | POST | /api/partner/corporate/setup-intent |  |  |  |  |  |
| 4 | Payment | Purchase credit | POST | /api/partner/corporate/purchase-credit |  |  |  |  |  |
| 5 | Invoice | Get default invoice setting | POST | /api/partner/corporate/get-default-invoice-setting |  |  |  |  |  |
| 6 | Invoice | Send invoice | POST | /api/partner/corporate/send-invoice |  |  |  |  |  |
| 7 | Data | Data purchase summary | GET | /api/partner/corporate/data-purchase |  |  |  |  |  |
| 8 | Top-up | User top-up | POST | /api/partner/corporate/users/{userId}/top-up |  |  |  |  |  |
| 9 | Top-up | Search top-up history | POST | /api/partner/corporate/search-top-up-histories |  |  |  |  |  |
| 10 | Top-up | Export top-up history | POST | /api/partner/corporate/export-top-up-histories |  |  |  |  |  |
| 11 | Refund | Search refund history | POST | /api/partner/corporate/search-refund-histories |  |  |  |  |  |
| 12 | Refund | Export refund history | POST | /api/partner/corporate/export-refund-histories |  |  |  |  |  |
| 13 | Correct | Correct missing top-up | POST | /api/partner/correct-missing-top-up-transaction |  |  |  |  |  |

#### A.6.4 Associate Management (Gift code assignment)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Search | Search assigned gift codes | POST | /api/partner/associate-management/assigned-gift-codes/search |  |  |  |  |  |
| 2 | Export | Export assigned gift codes | POST | /api/partner/associate-management/assigned-gift-codes/export |  |  |  |  |  |
| 3 | Assign | Assign gift codes | POST | /api/partner/associate-management/assigned-gift-codes/assign |  |  |  |  |  |
| 4 | Revoke | Revoke assigned gift codes | POST | /api/partner/associate-management/assigned-gift-codes/revoke |  |  |  |  |  |
| 5 | List | List associate accounts | GET | /api/partner/associate-management/assigned-gift-codes/get-list-associate |  |  |  |  |  |

#### A.6.5 Partner-with-us Onboarding

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Onboarding | Submit partner-with-us | POST | /api/partner/partner-with-us/create |  |  |  |  |  |
| 2 | Onboarding | Get partnership type | GET | /api/partner/partner-with-us/partnership-type |  |  |  |  |  |
| 3 | Misc | Get affiliate partners | GET | /api/partner/affiliate |  |  |  |  |  |
| 4 | Misc | Get link-based partners | GET | /api/partner/link-based-affiliate |  |  |  |  |  |
| 5 | Misc | Affiliate with network | GET | /api/partner/affiliate-have-network-program |  |  |  |  |  |

#### A.6.6 Partner Balances & Payout

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Payout | Search payouts | POST | /api/partner-balances/payouts/search |  |  |  |  |  |
| 2 | Payout | Get payout detail | GET | /api/partner-balances/payouts/get-details/{payoutId} |  |  |  |  |  |
| 3 | Payout | Export payouts | POST | /api/partner-balances/payouts/export |  |  |  |  |  |
| 4 | Payout | Pay commissions | POST | /api/partner-balances/pay-commissions |  |  |  |  |  |
| 5 | Payout | Get payouts in queue | GET | /api/partner-balances/get-payouts-in-queue |  |  |  |  |  |

#### A.6.7 Partner API Management

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | List | Get all API records | GET | /api/partner-api-management |  |  |  |  |  |
| 2 | Status | Change status | POST | /api/partner-api-management/change-status |  |  |  |  |  |
| 3 | Key | Change secret key | POST | /api/partner-api-management/change-key |  |  |  |  |  |
| 4 | Admin | Partner key by admin | GET | /api/partner-api-management/partner-key-by-admin/{partnerId} |  |  |  |  |  |
| 5 | Admin | Change status by admin | PUT | /api/partner-api-management/change-status-by-admin |  |  |  |  |  |

#### A.6.8 Partner Campaign Schedule Task

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create schedule | POST | /api/partner-campaign-schedule-task |  |  |  |  |  |
| 2 | CRUD | Update schedule | PUT | /api/partner-campaign-schedule-task |  |  |  |  |  |
| 3 | CRUD | Get schedule | GET | /api/partner-campaign-schedule-task |  |  |  |  |  |
| 4 | CRUD | Delete schedule | DELETE | /api/partner-campaign-schedule-task/{partnerCampaignScheduleTaskId} |  |  |  |  |  |
| 5 | List | Search schedules | POST | /api/partner-campaign-schedule-task/search |  |  |  |  |  |
| 6 | Test | Test schedule run | POST | /api/partner-campaign-schedule-task/test |  |  |  |  |  |

### A.7 Gift Code & Promotional Campaign

#### A.7.1 Gift Code (Shared / Unique / Sponsored)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create gift code | POST | /api/gift-code |  |  |  |  |  |
| 2 | Migration | Update short content format | POST | /api/gift-code/update-short-content-format |  |  |  |  |  |
| 3 | Validation | Campaign validation | POST | /api/gift-code/giftcode-validation |  |  |  |  |  |
| 4 | Update | Update sponsored share | PUT | /api/gift-code/update-sponsored-share |  |  |  |  |  |
| 5 | Update | Update unique gift | PUT | /api/gift-code/update-unique-gift |  |  |  |  |  |
| 6 | List | Search gift codes | POST | /api/gift-code/search |  |  |  |  |  |
| 7 | List | List campaigns | POST | /api/gift-code/list-campaigns |  |  |  |  |  |
| 8 | List | List campaigns v2 | POST | /api/gift-code/list-campaigns-v2 |  |  |  |  |  |
| 9 | Redeem | Search redeemed codes | POST | /api/gift-code/search-redeem |  |  |  |  |  |
| 10 | List | List gift | POST | /api/gift-code/list-gift |  |  |  |  |  |
| 11 | Export | Export by id | POST | /api/gift-code/export-by-id |  |  |  |  |  |
| 12 | Export | Export gift code | POST | /api/gift-code/export |  |  |  |  |  |
| 13 | Delete | Delete gift code | DELETE | /api/gift-code/{giftCodeId} |  |  |  |  |  |
| 14 | Delete | Delete multi | DELETE | /api/gift-code/delete-multi |  |  |  |  |  |
| 15 | Delete | Admin delete multi | DELETE | /api/gift-code/admin-delete-multi |  |  |  |  |  |
| 16 | Redeem | Redeem code | POST | /api/gift-code/redeem |  |  |  |  |  |
| 17 | Detail | Detail by gift code | GET | /api/gift-code/details/{giftCode} |  |  |  |  |  |
| 18 | Detail | Detail web | GET | /api/gift-code/details-web/{giftCode} |  |  |  |  |  |
| 19 | Status | Deactivate | PUT | /api/gift-code/deactivate |  |  |  |  |  |
| 20 | Validation | Check partner referral | GET | /api/gift-code/IsPartnerReferral/{code} |  |  |  |  |  |
| 21 | Redeem | Search redeem (end user) | POST | /api/gift-code/search-redeem-enduser |  |  |  |  |  |
| 22 | Email | Send gift code email | POST | /api/gift-code/send-giftcode-email |  |  |  |  |  |
| 23 | Unique | Update expired date | POST | /api/gift-code/unique/update-expired-date |  |  |  |  |  |
| 24 | Promo | Get promo codes (mobile) | GET | /api/gift-code/promo/get |  |  |  |  |  |
| 25 | Promo | Apply promo code | POST | /api/gift-code/promo/apply |  |  |  |  |  |
| 26 | Promo | Create promo v2 | POST | /api/gift-code/promo/v2 |  |  |  |  |  |
| 27 | Promo | Update promo v2 | PUT | /api/gift-code/promo/v2 |  |  |  |  |  |
| 28 | Promo | Search promo | POST | /api/gift-code/promo/search |  |  |  |  |  |
| 29 | Promo | Redeem manually | POST | /api/gift-code/promo/redeem-manually |  |  |  |  |  |
| 30 | Promo | Get promo v2 by id | GET | /api/gift-code/promo/v2/{Id} |  |  |  |  |  |
| 31 | Sponsor | Get sponsored gift | GET | /api/gift-code/sponsored-gift/{Id} |  |  |  |  |  |
| 32 | Promo | Delete promo | DELETE | /api/gift-code/promo |  |  |  |  |  |
| 33 | System | Add data to system account | POST | /api/gift-code/add-data-to-system-account |  |  |  |  |  |
| 34 | System | Get default code | GET | /api/gift-code/get-default-code |  |  |  |  |  |
| 35 | Unique | Delete by campaign | DELETE | /api/gift-code/unique-gift |  |  |  |  |  |
| 36 | Unique | List by campaign | GET | /api/gift-code/unique-gift |  |  |  |  |  |
| 37 | Unique | Edit by campaign | POST | /api/gift-code/unique-gift |  |  |  |  |  |
| 38 | Signup | Sign-up gift code | GET | /api/gift-code/sign-up-gift-code |  |  |  |  |  |
| 39 | Signup | Check sign-up gift code | POST | /api/gift-code/check-sign-up-gift-code |  |  |  |  |  |
| 40 | Mapping | Apply promo to data plans | GET | /api/gift-code/apply-promo-codes-to-dataplans |  |  |  |  |  |
| 41 | Promo | Get promo short content | GET | /api/gift-code/promo/short-content |  |  |  |  |  |

#### A.7.2 Promotional Campaign

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create campaign | POST | /api/promotional-campaign |  |  |  |  |  |
| 2 | CRUD | Update campaign | PUT | /api/promotional-campaign |  |  |  |  |  |
| 3 | CRUD | Delete campaign | DELETE | /api/promotional-campaign |  |  |  |  |  |
| 4 | Detail | Get by id | GET | /api/promotional-campaign/{Id} |  |  |  |  |  |
| 5 | List | Search campaigns | POST | /api/promotional-campaign/search |  |  |  |  |  |
| 6 | Section | Search member group | POST | /api/promotional-campaign/section/member-group |  |  |  |  |  |
| 7 | Section | Search promo code | POST | /api/promotional-campaign/section/promo-code |  |  |  |  |  |
| 8 | Section | Search notification | POST | /api/promotional-campaign/section/notification |  |  |  |  |  |
| 9 | Section | Search gift code | POST | /api/promotional-campaign/section/gift-code |  |  |  |  |  |
| 10 | Trigger | Trigger campaign | POST | /api/promotional-campaign/trigger |  |  |  |  |  |
| 11 | Popup | Get popup | GET | /api/promotional-campaign/popup |  |  |  |  |  |
| 12 | Popup | Close popup | POST | /api/promotional-campaign/popup/close |  |  |  |  |  |
| 13 | Promo | Promo code list | GET | /api/promotional-campaign/promo-code |  |  |  |  |  |
| 14 | Promo | Count promo codes | GET | /api/promotional-campaign/count-promo-code |  |  |  |  |  |
| 15 | Export | Export not-redeem | GET | /api/promotional-campaign/export/not-redeem |  |  |  |  |  |
| 16 | Export | Export not-use | GET | /api/promotional-campaign/export/not-use |  |  |  |  |  |
| 17 | Notify | Send notification redeem-not-use | POST | /api/promotional-campaign/send-notification/redeem-but-not-use |  |  |  |  |  |
| 18 | Email | Send test email | POST | /api/promotional-campaign/send-test-email |  |  |  |  |  |

### A.8 Payment & Withdrawal

#### A.8.1 Payment Methods (Admin manage)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | PayPal | Add PayPal account | POST | /api/setting-paypal-account |  |  |  |  |  |
| 2 | PayPal | Get PayPal accounts | GET | /api/setting-paypal-account |  |  |  |  |  |
| 3 | PayPal | Webhook | POST | /api/setting-paypal-account/webhook |  |  |  |  |  |
| 4 | WISE admin | List WISE admin | GET | /api/wisepayment/admin |  |  |  |  |  |
| 5 | WISE admin | Create WISE admin | POST | /api/wisepayment/admin |  |  |  |  |  |
| 6 | WISE partner | List WISE partner | GET | /api/wisepayment/partner |  |  |  |  |  |
| 7 | WISE partner | Create WISE partner | POST | /api/wisepayment/partner |  |  |  |  |  |
| 8 | Stripe | Get checkout details | POST | /api/v2.0/Payment/checkout-details |  |  |  |  |  |
| 9 | Stripe | Get Stripe payment methods | POST | /api/v2.0/Payment/stripe-payment-methods |  |  |  |  |  |
| 10 | Stripe | Webhook (intent) | POST | /api/v2.0/Payment/add-intent-stripe-webhook |  |  |  |  |  |
| 11 | Stripe | Webhook (warning) | POST | /api/v2.0/Payment/add-warning-stripe-webhook |  |  |  |  |  |
| 12 | Stripe | Intent payment details | GET | /api/v2.0/Payment/intent-payment-details |  |  |  |  |  |
| 13 | Stripe | Intent payment webhook | POST | /api/v2.0/Payment/intent-payment-webhook |  |  |  |  |  |
| 14 | Crypto | Crypto payment webhook | POST | /api/v2.0/Payment/crypto-payment-webhook |  |  |  |  |  |
| 15 | Stripe | Stripe warning webhook | POST | /api/v2.0/Payment/stripe-warning-webhook |  |  |  |  |  |
| 16 | Failure | Handle payment failed v2 | POST | /api/v2.0/Payment/handle-payment-when-failed-v2 |  |  |  |  |  |
| 17 | Failure | Handle payment failed v3 | POST | /api/v2.0/Payment/handle-payment-when-failed-v3 |  |  |  |  |  |
| 18 | Stripe | Auto correct user tx | POST | /api/v2.0/Payment/auto-correct-user-transaction |  |  |  |  |  |
| 19 | Stripe | Check intent payment status | POST | /api/v2.0/Payment/check-intent-payment-status |  |  |  |  |  |
| 20 | Stripe | Check add data on payment | GET | /api/v2.0/Payment/check-add-dataplan-on-payment |  |  |  |  |  |
| 21 | Stripe | Invoice email | GET | /api/v2.0/Payment/invoice-email |  |  |  |  |  |

#### A.8.2 TransferWise

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Wise | Add Wise account | POST | /api/v2.0/TransferWise |  |  |  |  |  |
| 2 | Wise | List by user | GET | /api/v2.0/TransferWise |  |  |  |  |  |
| 3 | Wise | All branch name | GET | /api/v2.0/TransferWise/AllBranchName |  |  |  |  |  |
| 4 | Wise | All bank name | GET | /api/v2.0/TransferWise/AllBankName |  |  |  |  |  |
| 5 | Wise | Withdrawal | POST | /api/v2.0/TransferWise/TransferWiseWithdrawal |  |  |  |  |  |
| 6 | Wise | Pay partner payout | POST | /api/v2.0/TransferWise/transfer-wise-payment-for-partner-payout |  |  |  |  |  |
| 7 | Wise | Pay commission | POST | /api/v2.0/TransferWise/TransferWisePayCommissionPayment |  |  |  |  |  |
| 8 | Wise | Pay referral | POST | /api/v2.0/TransferWise/TransferWisePayReferralPayment |  |  |  |  |  |
| 9 | Wise | Update payout status from wise | POST | /api/v2.0/TransferWise/update-partner-payout-status-from-wise-transfer |  |  |  |  |  |
| 10 | Wise | Update commission status | POST | /api/v2.0/TransferWise/UpdateCommissionPaymentStatusFromWiseTransfer |  |  |  |  |  |
| 11 | Wise | Update referral status | POST | /api/v2.0/TransferWise/UpdateReferralPaymentStatusFromWiseTransfer |  |  |  |  |  |
| 12 | Wise | Delete account | DELETE | /api/v2.0/TransferWise/{id} |  |  |  |  |  |
| 13 | Wise | Update all withdrawal status | POST | /api/v2.0/TransferWise/UpdateAllWithdrawalStatusOfTransferWise |  |  |  |  |  |
| 14 | Wise | Set up webhook | POST | /api/v2.0/TransferWise/Transferwise-set-up-webhook |  |  |  |  |  |
| 15 | Wise | Status webhook | POST | /api/v2.0/TransferWise/TransferWise-Change-Status-Webhook |  |  |  |  |  |
| 16 | Wise | Search currencies | POST | /api/v2.0/TransferWise/search-wise-currencies |  |  |  |  |  |

#### A.8.3 Withdrawal (Paypal)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Withdrawal | Request | POST | /api/v2.0/Withdrawal |  |  |  |  |  |
| 2 | Withdrawal | Search (mobile) | POST | /api/v2.0/Withdrawal/SearchWithdrawalInfo |  |  |  |  |  |
| 3 | Withdrawal | PayPal webhook | POST | /api/v2.0/Withdrawal/paypal-webhook |  |  |  |  |  |
| 4 | Withdrawal | Search (portal) | POST | /api/v2.0/Withdrawal/Search-portal |  |  |  |  |  |
| 5 | Withdrawal | Export | POST | /api/v2.0/Withdrawal/export |  |  |  |  |  |
| 6 | Withdrawal | Payment details | GET | /api/v2.0/Withdrawal/paymentDetails/{Id} |  |  |  |  |  |

### A.9 Content & Configuration

#### A.9.1 Content Pages

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create content page | POST | /api/content-page |  |  |  |  |  |
| 2 | CRUD | Update content page | PUT | /api/content-page |  |  |  |  |  |
| 3 | List | Search content pages | POST | /api/content-page/search |  |  |  |  |  |
| 4 | Mobile | Get content for customer | GET | /api/content-page/customer |  |  |  |  |  |
| 5 | Detail | Get by id | GET | /api/content-page/detail/{requestId} |  |  |  |  |  |
| 6 | Delete | Delete content page | DELETE | /api/content-page/{requestId} |  |  |  |  |  |
| 7 | Validation | Check content key | GET | /api/content-page/check-content-key/{contentKey} |  |  |  |  |  |
| 8 | Seed | Create app content seed | POST | /api/content-page/CreateAppContentSeed |  |  |  |  |  |
| 9 | Mobile | Mobile multi-language content | POST | /api/content-page/MobileMutiLanguageContent |  |  |  |  |  |
| 10 | Ecommerce | Ecommerce multi-language content | POST | /api/content-page/EcommerceMutiLanguageContent |  |  |  |  |  |
| 11 | Export | Export content pages | POST | /api/content-page/Export |  |  |  |  |  |
| 12 | Import | Import content pages | POST | /api/content-page/Import |  |  |  |  |  |
| 13 | Import | Search file import | POST | /api/content-page/search-file-import |  |  |  |  |  |
| 14 | Cache | Clear language cache | GET | /api/content-page/Cache-language |  |  |  |  |  |
| 15 | Cache | Get cache | GET | /api/content-page/Get-cache |  |  |  |  |  |
| 16 | Notification | Transaction content notification | GET | /api/content-page/transaction-content/notification |  |  |  |  |  |
| 17 | Duplicate | Duplicate API content | POST | /api/content-page/duplicate-api-content |  |  |  |  |  |
| 18 | Seed | Overlength seed | POST | /api/content-page/overlength-seed |  |  |  |  |  |

#### A.9.2 App Config Images

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create image config | POST | /api/appconfig-image |  |  |  |  |  |
| 2 | CRUD | Update image config | PUT | /api/appconfig-image |  |  |  |  |  |
| 3 | List | Search image configs | POST | /api/appconfig-image/search |  |  |  |  |  |
| 4 | Delete | Delete image config | DELETE | /api/appconfig-image/{ImageConfigId} |  |  |  |  |  |
| 5 | Detail | Get by id | GET | /api/appconfig-image/{ImageConfigId} |  |  |  |  |  |
| 6 | Validation | Check image key | GET | /api/appconfig-image/imagekey-check/{imageKey} |  |  |  |  |  |

#### A.9.3 Help Content

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create help content | POST | /api/HelpContent |  |  |  |  |  |
| 2 | CRUD | Update help content | PUT | /api/HelpContent |  |  |  |  |  |
| 3 | Detail | Get by id | GET | /api/HelpContent/{helpContentId} |  |  |  |  |  |
| 4 | Detail | Get by module position | GET | /api/HelpContent/position/{moduleId} |  |  |  |  |  |
| 5 | Detail | Get attached content | GET | /api/HelpContent/attached-content/{helpContentId} |  |  |  |  |  |
| 6 | List | Search help content | POST | /api/HelpContent/search |  |  |  |  |  |
| 7 | Delete | Delete help content | DELETE | /api/HelpContent/{HelpContentId} |  |  |  |  |  |
| 8 | Partner | Get partner help | GET | /api/HelpContent/partner |  |  |  |  |  |

#### A.9.4 Application Notifications

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create notification | POST | /api/application/notifications |  |  |  |  |  |
| 2 | CRUD | Update notification | PUT | /api/application/notifications |  |  |  |  |  |
| 3 | List | Search notifications | POST | /api/application/notifications/Search |  |  |  |  |  |
| 4 | Delete | Delete notification | DELETE | /api/application/notifications/{id} |  |  |  |  |  |
| 5 | Detail | Get by id | GET | /api/application/notifications/{id} |  |  |  |  |  |
| 6 | Tooltip | Get tooltip by id | GET | /api/application/notifications/tooltip/{id} |  |  |  |  |  |
| 7 | Link | Check link to guest notification | GET | /api/application/notifications/check-link-to-guest-notification/{id} |  |  |  |  |  |
| 8 | Firebase | Get user notification preference | POST | /api/v2.0/Notifications/GetNotificationUserPreference |  |  |  |  |  |
| 9 | Firebase | Save user notification preference | POST | /api/v2.0/Notifications/PostNotificationUserPreference |  |  |  |  |  |
| 10 | Firebase | Save firebase token | POST | /api/v2.0/Notifications/SaveFirebaseToken |  |  |  |  |  |
| 11 | Firebase | New seed topic | POST | /api/v2.0/Notifications/new-seed-topic |  |  |  |  |  |
| 12 | Firebase | Check firebase token | GET | /api/v2.0/Notifications/check-firebase-token |  |  |  |  |  |

#### A.9.5 Guest Notifications

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create guest notification | POST | /api/guest-notification |  |  |  |  |  |
| 2 | CRUD | Update guest notification | PUT | /api/guest-notification |  |  |  |  |  |
| 3 | CRUD | Delete guest notification | DELETE | /api/guest-notification |  |  |  |  |  |
| 4 | List | Search guest notifications | POST | /api/guest-notification/search |  |  |  |  |  |
| 5 | Detail | Get by id | GET | /api/guest-notification/{Id} |  |  |  |  |  |
| 6 | Status | Update status | POST | /api/guest-notification/update-status |  |  |  |  |  |
| 7 | Search | Search notifications (mobile) | POST | /api/guest-notification/search-notification |  |  |  |  |  |
| 8 | Validation | Check duplicate name | GET | /api/guest-notification/check-duplicate-name/{name} |  |  |  |  |  |

#### A.9.6 Translation Queue

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Bulk | Bulk translate content | POST | /api/Translate/bulk-content |  |  |  |  |  |
| 2 | Queue | Get system queue by id | GET | /api/Translate/bulk-content-queue/system/{id} |  |  |  |  |  |
| 3 | Queue | Get notification queue by id | GET | /api/Translate/bulk-content-queue/notification/{id} |  |  |  |  |  |
| 4 | Queue | Get CMS queue by id | GET | /api/Translate/bulk-content-queue/cms/{id} |  |  |  |  |  |
| 5 | Queue | Get appconfig-image queue by id | GET | /api/Translate/bulk-content-queue/appconfig-image/{id} |  |  |  |  |  |
| 6 | Queue | Trigger bulk translate | POST | /api/Translate/bulk-content-queue/trigger |  |  |  |  |  |
| 7 | Retry | Retry content queue | POST | /api/Translate/content-queue/retry-trigger |  |  |  |  |  |
| 8 | Search | Search bulk content queue | POST | /api/Translate/bulk-content-queue/search |  |  |  |  |  |
| 9 | Delete | Delete content queue | DELETE | /api/Translate/content-queue |  |  |  |  |  |

#### A.9.7 Translation Prompts

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | List | Search translation prompts | POST | /api/translation-prompt/search |  |  |  |  |  |
| 2 | CRUD | Create prompt | POST | /api/translation-prompt |  |  |  |  |  |
| 3 | CRUD | Update prompt | PUT | /api/translation-prompt |  |  |  |  |  |
| 4 | Detail | Get by id | GET | /api/translation-prompt/{id} |  |  |  |  |  |
| 5 | Delete | Delete prompt | DELETE | /api/translation-prompt/{id} |  |  |  |  |  |

#### A.9.8 Master Data & Cache

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Master | Get country list | GET | /api/v2.0/Master/CountryList |  |  |  |  |  |
| 2 | Master | Clear country history | PUT | /api/v2.0/Master/clear-Country-history |  |  |  |  |  |
| 3 | Master | Get currency list | GET | /api/v2.0/Master/CurrencyList |  |  |  |  |  |
| 4 | Master | Get currency codes | GET | /api/v2.0/Master/CurrencyCodes |  |  |  |  |  |
| 5 | Master | Get language list | GET | /api/v2.0/Master/LanguageList |  |  |  |  |  |
| 6 | Cache | Clear all cache | PUT | /api/v2.0/Master/clear-cache |  |  |  |  |  |
| 7 | Maintenance | Check maintenance by type | GET | /api/v2.0/Master/is-maintenance/{type} |  |  |  |  |  |
| 8 | Cache | Clear cache by key | POST | /api/v2.0/Master/clear-cache-by-key |  |  |  |  |  |
| 9 | Test | Test partner API | GET | /api/v2.0/Master/Test-partner-api |  |  |  |  |  |
| 10 | Tracking | Create failed tracking | POST | /api/v2.0/Master/create-failed-tracking |  |  |  |  |  |
| 11 | Log | Clear RR log | GET | /api/v2.0/Master/clear-rr-log |  |  |  |  |  |
| 12 | Migration | Add country code id for member | GET | /api/v2.0/Master/add-country-code-id-for-member |  |  |  |  |  |
| 13 | Cache | Clear cache by key part | POST | /api/v2.0/Master/clear-cache-by-key-part |  |  |  |  |  |
| 14 | Debug | Get SingTel request id | GET | /api/v2.0/Master/singtel-request-id |  |  |  |  |  |
| 15 | Queue | Add user-id to activated dataplan queue | POST | /api/v2.0/Master/add-user-id-to-actiaved-dataplan-queue |  |  |  |  |  |
| 16 | Debug | Get eSim DB | GET | /api/v2.0/Master/esimdb |  |  |  |  |  |
| 17 | Seed | Referral count seed data | GET | /api/v2.0/Master/referal-count-seed-data |  |  |  |  |  |
| 18 | Security | Set security config | POST | /api/v2.0/Master/set-security-cofig |  |  |  |  |  |
| 19 | Config | Set change mobile signup number limit | POST | /api/v2.0/Master/set-change-mobile-signup-number-limit |  |  |  |  |  |
| 20 | Config | Update file block info | POST | /api/v2.0/Master/update-file-block-info |  |  |  |  |  |
| 21 | Migration | Update user dataplan from job | POST | /api/v2.0/Master/update-user-dataplan-from-job |  |  |  |  |  |
| 22 | Config | Switch referral program flow | POST | /api/v2.0/Master/referral-program/switch-flow |  |  |  |  |  |
| 23 | Currency | Search currency | POST | /api/v2.0/Master/search-currency |  |  |  |  |  |

#### A.9.9 Languages Management

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | List | Search master languages | POST | /api/master-language/search |  |  |  |  |  |
| 2 | CRUD | Create language | POST | /api/master-language/create |  |  |  |  |  |
| 3 | Detail | Get by id | GET | /api/master-language/{masterLanguageId} |  |  |  |  |  |
| 4 | Delete | Delete language | DELETE | /api/master-language/{masterLanguageId} |  |  |  |  |  |
| 5 | CRUD | Update language | PUT | /api/master-language |  |  |  |  |  |

#### A.9.10 Global Settings

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Settings | Get global settings | GET | /api/GlobalSettings |  |  |  |  |  |
| 2 | Settings | Save global settings | POST | /api/GlobalSettings |  |  |  |  |  |

#### A.9.11 Currencies Management (FX Rates)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | FX | Get round-up options | GET | /api/currencies-management/fx-rates/round-up-options |  |  |  |  |  |
| 2 | Currency | Get currency codes | GET | /api/currencies-management/currencies/codes |  |  |  |  |  |
| 3 | Currency | Search currencies | POST | /api/currencies-management/currencies/search |  |  |  |  |  |
| 4 | Currency | Update currency | PUT | /api/currencies-management/currencies/update |  |  |  |  |  |
| 5 | FX | Search FX rates | POST | /api/currencies-management/fx-rates/search |  |  |  |  |  |
| 6 | FX | Update FX rate | PUT | /api/currencies-management/fx-rates/update |  |  |  |  |  |
| 7 | FX | Create FX rate | POST | /api/currencies-management/fx-rates/create |  |  |  |  |  |
| 8 | FX | Convert exchange rate | GET | /api/currencies-management/exchange-rate/convert |  |  |  |  |  |

#### A.9.12 Version Management

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create version | POST | /api/version-management |  |  |  |  |  |
| 2 | CRUD | Update version | PUT | /api/version-management |  |  |  |  |  |
| 3 | List | Search versions | POST | /api/version-management/search |  |  |  |  |  |
| 4 | Detail | Get by id | GET | /api/version-management/{id} |  |  |  |  |  |
| 5 | Check | Check version | GET | /api/version-management/check-version |  |  |  |  |  |
| 6 | Delete | Delete version | DELETE | /api/version-management/{requestId} |  |  |  |  |  |
| 7 | Mobile | Check version on mobile | GET | /api/version-management/CheckVersionOnMobile |  |  |  |  |  |

#### A.9.13 Maintenance Schedule

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create maintenance schedule | POST | /api/MaintenanceSchedule |  |  |  |  |  |
| 2 | CRUD | Update maintenance schedule | POST | /api/MaintenanceSchedule/update |  |  |  |  |  |
| 3 | List | Search schedules | POST | /api/MaintenanceSchedule/search |  |  |  |  |  |
| 4 | Delete | Delete schedule | DELETE | /api/MaintenanceSchedule/{maintenanceId} |  |  |  |  |  |
| 5 | Refresh | Refresh schedule | POST | /api/MaintenanceSchedule/refesh |  |  |  |  |  |

#### A.9.14 Link Management

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | List | Search links | POST | /api/LinkManagements/search |  |  |  |  |  |
| 2 | CRUD | Create link | POST | /api/LinkManagements/link |  |  |  |  |  |
| 3 | CRUD | Update link | PUT | /api/LinkManagements/link |  |  |  |  |  |
| 4 | Custom | Create custom link | POST | /api/LinkManagements/custom-link |  |  |  |  |  |
| 5 | Delete | Delete link | DELETE | /api/LinkManagements/link/{linkId} |  |  |  |  |  |

#### A.9.15 Email Notification Templates

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Email | Send hear-from-you email | POST | /api/EmailNotification/SendHearFromYouEmail |  |  |  |  |  |
| 2 | Email | Data transfer - recipient notify | POST | /api/EmailNotification/DataTransfer/Recipient |  |  |  |  |  |
| 3 | Email | Data transfer - sender notify | POST | /api/EmailNotification/DataTransfer/Sender |  |  |  |  |  |
| 4 | Email | Send receipt email | POST | /api/EmailNotification/SendReceipt |  |  |  |  |  |
| 5 | Email | Send redeem email | POST | /api/EmailNotification/SendRedeemEmail |  |  |  |  |  |
| 6 | Email | Send email after sign-up | POST | /api/EmailNotification/SendEmailAfterSignUp |  |  |  |  |  |
| 7 | Email | Send 500MB reward email | POST | /api/EmailNotification/SendEmail500MBReward |  |  |  |  |  |
| 8 | Email | Send first active plan email | POST | /api/EmailNotification/Send-first-acitve-plan |  |  |  |  |  |
| 9 | Email | Send multi dynamic email | POST | /api/EmailNotification/Send-multi-dynamic-email |  |  |  |  |  |
| 10 | Template | Search SendGrid templates | POST | /api/EmailNotification/search-send-grid-template |  |  |  |  |  |

---

### A.10 Security & Fraud

#### A.10.1 Fraud Alert

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Seed | Seed fraud alerts | POST | /api/frauds-alert/seed |  |  |  |  |  |
| 2 | Filter | Get filter constants | GET | /api/frauds-alert/FilterConstant |  |  |  |  |  |
| 3 | CRUD | Create fraud alert | POST | /api/frauds-alert |  |  |  |  |  |
| 4 | CRUD | Update fraud alert | PUT | /api/frauds-alert |  |  |  |  |  |
| 5 | List | Get all fraud alerts | GET | /api/frauds-alert |  |  |  |  |  |
| 6 | Delete | Delete fraud alert | DELETE | /api/frauds-alert |  |  |  |  |  |
| 7 | Alert | Edit last update alert | PUT | /api/frauds-alert/edit-last-update-alert |  |  |  |  |  |
| 8 | Seed | Seed transaction alert | POST | /api/frauds-alert/seed-transaction |  |  |  |  |  |
| 9 | Filter | Get transaction constants | GET | /api/frauds-alert/transaction-constant |  |  |  |  |  |

#### A.10.2 Warning Stripe

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Events | Search Stripe events | POST | /api/WarningStripe/search-stripe-event |  |  |  |  |  |
| 2 | Rules | Search review rules | POST | /api/WarningStripe/search-review-rule |  |  |  |  |  |
| 3 | Rules | Add review rule | POST | /api/WarningStripe/add-review-rule |  |  |  |  |  |
| 4 | Rules | Update review rule | PUT | /api/WarningStripe/update-review-rule |  |  |  |  |  |
| 5 | Rules | Delete review rule | DELETE | /api/WarningStripe/review-rule |  |  |  |  |  |
| 6 | Action | Do action on event | POST | /api/WarningStripe/do-action |  |  |  |  |  |
| 7 | Reason | Get reason events | POST | /api/WarningStripe/reason-event |  |  |  |  |  |

#### A.10.3 Block Email Pattern

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Add email pattern | POST | /api/BlockEmailPattern |  |  |  |  |  |
| 2 | Delete | Delete email pattern | DELETE | /api/BlockEmailPattern |  |  |  |  |  |
| 3 | Seed | Import seed data | POST | /api/BlockEmailPattern/import-seed-data |  |  |  |  |  |
| 4 | List | Search email patterns | POST | /api/BlockEmailPattern/search |  |  |  |  |  |

#### A.10.4 Whitelist

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | List | Search whitelist | POST | /api/whitelist/search |  |  |  |  |  |

#### A.10.5 SMS Configuration

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Config | Create SMS config | POST | /api/sms |  |  |  |  |  |
| 2 | Config | Get SMS config | GET | /api/sms |  |  |  |  |  |
| 3 | Rate limit | Get rate limit config | GET | /api/sms/rate-limit-config |  |  |  |  |  |
| 4 | Rate limit | Add rate limit config | POST | /api/sms/add-rate-limit-config |  |  |  |  |  |
| 5 | Rate limit | Get rate limit config detail | GET | /api/sms/get-rate-limit-config |  |  |  |  |  |
| 6 | Block | Manual block OTP | POST | /api/sms/manual-block-otp |  |  |  |  |  |
| 7 | Block | Get block OTP status | GET | /api/sms/get-block-otp-status |  |  |  |  |  |
| 8 | Blacklist | Add blacklist config | POST | /api/sms/add-black-list-config |  |  |  |  |  |
| 9 | Blacklist | Get blacklist config | GET | /api/sms/get-black-list-config |  |  |  |  |  |
| 10 | OTP | Generate OTP (mobile) | POST | /api/v2.0/SMS/GenrateOTP |  |  |  |  |  |
| 11 | OTP | Generate OTP (web) | POST | /api/v2.0/SMS/GenrateOTP-Web |  |  |  |  |  |
| 12 | Webhook | Twilio webhook | POST | /api/v2.0/SMS/twilio-webhook |  |  |  |  |  |
| 13 | Webhook | Telesign webhook | POST | /api/v2.0/SMS/telesign-webhook |  |  |  |  |  |
| 14 | Webhook | Vonage webhook | GET | /api/v2.0/SMS/vonage-webhook |  |  |  |  |  |
| 15 | OTP | Add record for new OTP status flow | POST | /api/v2.0/SMS/Add-Record-For-New-Otp-Status-Flow |  |  |  |  |  |
| 16 | WhatsApp | WhatsApp webhook (POST) | POST | /api/v2.0/SMS/whatsapp-webhook |  |  |  |  |  |
| 17 | WhatsApp | WhatsApp webhook (GET) | GET | /api/v2.0/SMS/whatsapp-webhook |  |  |  |  |  |
| 18 | OTP | Worker generate OTP | POST | /api/v2.0/SMS/Worker/GenrateOTP |  |  |  |  |  |
| 19 | OTP | Worker generate OTP (web) | POST | /api/v2.0/SMS/Worker/GenrateOTP-Web |  |  |  |  |  |
| 20 | OTP | Email generate OTP | POST | /api/v2.0/SMS/Email/GenrateOTP |  |  |  |  |  |
| 21 | OTP | Email generate OTP (web) | POST | /api/v2.0/SMS/Email/GenrateOTP-Web |  |  |  |  |  |
| 22 | Verify | Verify mobile number | POST | /api/v2.0/SMS/verify-mobile-number |  |  |  |  |  |

#### A.10.6 WhatsApp Management

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | CRUD | Create WhatsApp config | POST | /api/whatsapp |  |  |  |  |  |
| 2 | CRUD | Update WhatsApp config | PUT | /api/whatsapp |  |  |  |  |  |
| 3 | Delete | Delete WhatsApp config | DELETE | /api/whatsapp |  |  |  |  |  |
| 4 | List | Search WhatsApp configs | POST | /api/whatsapp/search |  |  |  |  |  |

---

## B. PARTNER WEB PORTAL

> Các endpoint partner portal sử dụng — bao gồm đăng nhập, dashboard, bán hàng, hoa hồng, payout.

### B.1 Authentication

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Login | Partner send OTP | POST | /api/partner/login |  |  |  |  |  |
| 2 | Login | Partner verify OTP & get token | POST | /api/partner/otp |  |  |  |  |  |
| 3 | OTP | Verify OTP (mobile) | POST | /api/v2.0/tokens/VerifyOTP |  |  |  |  |  |
| 4 | OTP | Verify OTP (web) | POST | /api/v2.0/tokens/VerifyOTP-Web |  |  |  |  |  |
| 5 | OTP | Worker verify OTP | POST | /api/v2.0/tokens/Worker/VerifyOTP |  |  |  |  |  |
| 6 | OTP | Worker verify OTP (web) | POST | /api/v2.0/tokens/Worker/VerifyOTP-Web |  |  |  |  |  |
| 7 | Role | Get partner role permissions | GET | /api/account-permision/partnership/role-permisions |  |  |  |  |  |

### B.2 Dashboard

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Dashboard | Partner dashboard summary | POST | /api/partner/dashboard |  |  |  |  |  |
| 2 | Dashboard | Available metric items | GET | /api/partner/metric-items-available |  |  |  |  |  |
| 3 | Metrics | Aggregate metrics | POST | /api/dashboard/metrics |  |  |  |  |  |
| 4 | View | Get user settings | GET | /api/dashboard/get-user-settings |  |  |  |  |  |
| 5 | View | Group by view data | POST | /api/dashboard/group-by-view |  |  |  |  |  |
| 6 | View | Upsert group by item | POST | /api/dashboard/upsert-group-by-view-item |  |  |  |  |  |
| 7 | Membership | Membership by country (partner) | POST | /api/dashboard/membership/country/refresh-partner-data |  |  |  |  |  |
| 8 | Membership | Membership by gender (partner) | POST | /api/dashboard/membership/genders/refresh-partner-data |  |  |  |  |  |
| 9 | Membership | Membership by age (partner) | POST | /api/dashboard/membership/age-users/refresh-partner-data |  |  |  |  |  |
| 10 | Membership | Use esims (partner) | POST | /api/dashboard/membership/use-esims/refresh-partner-data |  |  |  |  |  |

### B.3 Partner Sale — Post-paid (Data Plan)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | eSim | Activate eSim (portal) | POST | /api/Esims/activate-esim-portal |  |  |  |  |  |
| 2 | eSim | Bulk activate eSim (portal) | POST | /api/Esims/bulk-activate-esim-portal |  |  |  |  |  |
| 3 | eSim | Add data plan (portal) | POST | /api/Esims/add-data-plan-portal |  |  |  |  |  |
| 4 | Data plan | Search BSS data plans | POST | /api/Esims/search-bss-data-plans |  |  |  |  |  |
| 5 | Worker | Search workers | POST | /api/partner/worker-management/search |  |  |  |  |  |
| 6 | Worker | Worker profile | GET | /api/partner/worker-management/users/{userId}/profile |  |  |  |  |  |
| 7 | Worker | Worker eSim profile | GET | /api/partner/worker-management/users/{userId}/esim-profile |  |  |  |  |  |
| 8 | History | Search payment history | POST | /api/partner/corporate/search-payment-history |  |  |  |  |  |
| 9 | History | Export payment history | POST | /api/partner/corporate/payment-history/export |  |  |  |  |  |
| 10 | Top-up | Top-up user | POST | /api/partner/corporate/users/{userId}/top-up |  |  |  |  |  |
| 11 | Top-up | Search top-up history | POST | /api/partner/corporate/search-top-up-histories |  |  |  |  |  |
| 12 | Top-up | Export top-up history | POST | /api/partner/corporate/export-top-up-histories |  |  |  |  |  |
| 13 | Refund | Search refund history | POST | /api/partner/corporate/search-refund-histories |  |  |  |  |  |
| 14 | Refund | Export refund history | POST | /api/partner/corporate/export-refund-histories |  |  |  |  |  |
| 15 | Invoice | Get default invoice setting | POST | /api/partner/corporate/get-default-invoice-setting |  |  |  |  |  |
| 16 | Invoice | Send invoice | POST | /api/partner/corporate/send-invoice |  |  |  |  |  |
| 17 | Credit | Purchase credit | POST | /api/partner/corporate/purchase-credit |  |  |  |  |  |
| 18 | Credit | Setup intent | POST | /api/partner/corporate/setup-intent |  |  |  |  |  |
| 19 | Data | Data purchase summary | GET | /api/partner/corporate/data-purchase |  |  |  |  |  |

### B.4 Partner Sale — Pre-paid & Voucher (Consignment)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Consignment | Create consignment payment | POST | /api/consignment |  |  |  |  |  |
| 2 | Consignment | Import sold voucher | POST | /api/consignment/import-sold-voucher |  |  |  |  |  |
| 3 | Consignment | Search consignment | POST | /api/consignment/search |  |  |  |  |  |
| 4 | Consignment | Export consignment | POST | /api/consignment/export |  |  |  |  |  |
| 5 | Consignment | Pay consignment | POST | /api/consignment/pay |  |  |  |  |  |
| 6 | Consignment | Notification payment | GET | /api/consignment/notification-payment |  |  |  |  |  |
| 7 | Invoice | Search invoices | POST | /api/invoice/search |  |  |  |  |  |
| 8 | Invoice | Export invoice | POST | /api/invoice/export |  |  |  |  |  |
| 9 | Invoice | Export voucher of invoice | GET | /api/invoice/export-voucher/{invoiceId} |  |  |  |  |  |
| 10 | Voucher | Search voucher | POST | /api/voucher/search |  |  |  |  |  |
| 11 | Voucher | Export voucher | POST | /api/voucher/Export |  |  |  |  |  |

### B.5 Partner Sale — Gift Code

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Gift code | List campaigns | POST | /api/gift-code/list-campaigns |  |  |  |  |  |
| 2 | Gift code | List campaigns v2 | POST | /api/gift-code/list-campaigns-v2 |  |  |  |  |  |
| 3 | Gift code | List gift codes | POST | /api/gift-code/list-gift |  |  |  |  |  |
| 4 | Gift code | Search redeemed codes | POST | /api/gift-code/search-redeem |  |  |  |  |  |
| 5 | Gift code | Export by id | POST | /api/gift-code/export-by-id |  |  |  |  |  |
| 6 | Gift code | Get sponsored gift | GET | /api/gift-code/sponsored-gift/{Id} |  |  |  |  |  |
| 7 | Gift code | Get promo codes (mobile) | GET | /api/gift-code/promo/get |  |  |  |  |  |
| 8 | Associate | Search assigned gift codes | POST | /api/partner/associate-management/assigned-gift-codes/search |  |  |  |  |  |
| 9 | Associate | Export assigned gift codes | POST | /api/partner/associate-management/assigned-gift-codes/export |  |  |  |  |  |
| 10 | Associate | Assign gift codes | POST | /api/partner/associate-management/assigned-gift-codes/assign |  |  |  |  |  |
| 11 | Associate | Revoke assigned gift codes | POST | /api/partner/associate-management/assigned-gift-codes/revoke |  |  |  |  |  |
| 12 | Associate | List associate accounts | GET | /api/partner/associate-management/assigned-gift-codes/get-list-associate |  |  |  |  |  |

### B.6 Commission & Payout

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Commission | Search commission | POST | /api/commission/search |  |  |  |  |  |
| 2 | Commission | Request withdrawal | POST | /api/commission/request-withdrawal |  |  |  |  |  |
| 3 | Referral | Partner commission search | POST | /api/referral-general/referral-commission-partner/search |  |  |  |  |  |
| 4 | Referral | Partner payment search | POST | /api/referral-general/referral-payment-partner/search |  |  |  |  |  |
| 5 | Referral | Export partner commission | POST | /api/referral-general/referral-commission-partner/export |  |  |  |  |  |
| 6 | Referral | Export partner payment | POST | /api/referral-general/referral-payment-partner/export |  |  |  |  |  |
| 7 | Payout | Search payouts | POST | /api/partner-balances/payouts/search |  |  |  |  |  |
| 8 | Payout | Get payout detail | GET | /api/partner-balances/payouts/get-details/{payoutId} |  |  |  |  |  |
| 9 | Payout | Export payouts | POST | /api/partner-balances/payouts/export |  |  |  |  |  |
| 10 | WISE | List WISE partner | GET | /api/wisepayment/partner |  |  |  |  |  |
| 11 | WISE | Create WISE partner | POST | /api/wisepayment/partner |  |  |  |  |  |
| 12 | TransferWise | Add Wise account | POST | /api/v2.0/TransferWise |  |  |  |  |  |
| 13 | TransferWise | List Wise accounts | GET | /api/v2.0/TransferWise |  |  |  |  |  |

### B.7 Partner Settings & User Management

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Profile | Get partner v2 | GET | /api/partner/v2/{partnerId} |  |  |  |  |  |
| 2 | Profile | Update partner v2 | PUT | /api/partner/v2 |  |  |  |  |  |
| 3 | API key | Get partner key | GET | /api/partner-api-management/partner-key-by-admin/{partnerId} |  |  |  |  |  |
| 4 | API key | Change secret key | POST | /api/partner-api-management/change-key |  |  |  |  |  |
| 5 | API key | Change status | POST | /api/partner-api-management/change-status |  |  |  |  |  |
| 6 | Worker | Create worker | POST | /api/partner/worker-management |  |  |  |  |  |
| 7 | Worker | Update worker | PUT | /api/partner/worker-management |  |  |  |  |  |
| 8 | Worker | Update worker status | POST | /api/partner/worker-management/update-status |  |  |  |  |  |
| 9 | Worker | Bulk top-up | POST | /api/partner/worker-management/bulk-top-up |  |  |  |  |  |
| 10 | Import | Validate import | POST | /api/partner/worker-management/validation-import |  |  |  |  |  |
| 11 | Import | Confirm import | POST | /api/partner/worker-management/confirm-import |  |  |  |  |  |
| 12 | Self top-up | Get self top-up setting | GET | /api/partner/worker-management/self-topup/{userId} |  |  |  |  |  |
| 13 | Self top-up | Update self top-up | PUT | /api/partner/worker-management/self-topup |  |  |  |  |  |
| 14 | Setting | Set default setting | POST | /api/partner/set-default-setting |  |  |  |  |  |
| 15 | Setting | Get default setting | GET | /api/partner/get-default-setting |  |  |  |  |  |

---

## C. CUSTOMER MOBILE APP

> Các endpoint dành cho ứng dụng di động của khách hàng.

### C.1 Authentication & OTP

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Sign-up | Sign up (mobile) | POST | /api/v2.0/Customer/SignUp |  |  |  |  |  |
| 2 | Sign-up | Sign up (email) | POST | /api/v2.0/Customer/Email/SignUp |  |  |  |  |  |
| 3 | Sign-up | Worker sign up | POST | /api/v2.0/Customer/Worker/SignUp |  |  |  |  |  |
| 4 | Sign-up | Worker sign-up form | GET | /api/v2.0/Customer/WorkerSignUpForm/{email} |  |  |  |  |  |
| 5 | OTP | Generate OTP (mobile) | POST | /api/v2.0/SMS/GenrateOTP |  |  |  |  |  |
| 6 | OTP | Generate OTP (web) | POST | /api/v2.0/SMS/GenrateOTP-Web |  |  |  |  |  |
| 7 | OTP | Verify OTP (mobile) | POST | /api/v2.0/tokens/VerifyOTP |  |  |  |  |  |
| 8 | OTP | Verify OTP (web) | POST | /api/v2.0/tokens/VerifyOTP-Web |  |  |  |  |  |
| 9 | OTP | Email generate OTP | POST | /api/v2.0/SMS/Email/GenrateOTP |  |  |  |  |  |
| 10 | OTP | Email verify OTP | POST | /api/v2.0/tokens/Email/VerifyOTP |  |  |  |  |  |
| 11 | OTP | Agent verify OTP | POST | /api/v2.0/tokens/VerifyOTP-Agent |  |  |  |  |  |
| 12 | Verify | Verify mobile number via SMS | POST | /api/v2.0/SMS/verify-mobile-number |  |  |  |  |  |
| 13 | Mobile | Is mobile verified | GET | /api/v2.0/Customer/is-mobile-verified |  |  |  |  |  |
| 14 | Mobile | Verify mobile number | POST | /api/v2.0/Customer/verify-mobile-number |  |  |  |  |  |
| 15 | Social | Social auth (ecommerce) | POST | /api/social-authentication/ecommerce |  |  |  |  |  |
| 16 | Social | Social auth (mobile) | POST | /api/social-authentication/mobile |  |  |  |  |  |
| 17 | Social | Social sign up | POST | /api/social-authentication/SignUp |  |  |  |  |  |
| 18 | Social | Link to mobile account | POST | /api/social-authentication/link-to-mobile-account |  |  |  |  |  |
| 19 | Social | Get social list | GET | /api/v2.0/Customer/social-list |  |  |  |  |  |
| 20 | Social | Unlink social account | POST | /api/v2.0/Customer/unlinked-social-account |  |  |  |  |  |

### C.2 Profile & Account

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Profile | Get profile | GET | /api/v2.0/Customer/Profile |  |  |  |  |  |
| 2 | Profile | Update profile | POST | /api/v2.0/Customer/Profile |  |  |  |  |  |
| 3 | Profile | Upload profile image | POST | /api/v2.0/Customer/PostCustomerProfileImage |  |  |  |  |  |
| 4 | Profile | Delete profile image | POST | /api/v2.0/Customer/DeleteProfileImage |  |  |  |  |  |
| 5 | Profile | Update currency | PUT | /api/v2.0/Customer/update-currency |  |  |  |  |  |
| 6 | Profile | Update language | POST | /api/v2.0/Customer/update-language |  |  |  |  |  |
| 7 | Profile | Update language id | POST | /api/v2.0/Customer/update-language-id |  |  |  |  |  |
| 8 | Account | Delete account | DELETE | /api/v2.0/Customer/DeleteUser |  |  |  |  |  |
| 9 | Account | Search delete requests | POST | /api/v2.0/Customer/delete-user-requests/search |  |  |  |  |  |
| 10 | Account | Get delete notification | GET | /api/v2.0/Customer/delete-user-requests/get-notification |  |  |  |  |  |
| 11 | Account | Retry delete requests | POST | /api/v2.0/Customer/delete-user-requests/retry |  |  |  |  |  |
| 12 | Account | Find by phone number | POST | /api/v2.0/Customer/find-by-phone-number |  |  |  |  |  |
| 13 | Account | Update open app time | POST | /api/v2.0/Customer/UpdateOpenAppTime |  |  |  |  |  |
| 14 | Account | Count active user daily | POST | /api/v2.0/Customer/CountActiveUserDaily |  |  |  |  |  |
| 15 | Feedback | Review popup check | POST | /api/v2.0/Customer/reviewpopup/check |  |  |  |  |  |
| 16 | Feedback | Review popup opt-out | POST | /api/v2.0/Customer/reviewpopup/optout |  |  |  |  |  |
| 17 | Feedback | Submit feedback | POST | /api/v2.0/Customer/feedback |  |  |  |  |  |
| 18 | Buyer | Check first time buyer | POST | /api/v2.0/Customer/check-first-time-buyer |  |  |  |  |  |
| 19 | Reset | Reset eKYC | GET | /api/v2.0/Customer/reset-ekyc |  |  |  |  |  |
| 20 | Reset | Reset mobile account | GET | /api/v2.0/Customer/reset-mobile-account |  |  |  |  |  |
| 21 | Reset | Reset social account | GET | /api/v2.0/Customer/reset-social-account |  |  |  |  |  |
| 22 | Mobile detail | Get mobile user detail | GET | /api/v2.0/Customer/MobileUserDetail |  |  |  |  |  |

### C.3 eKYC (Jumio & Singpass)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Jumio | Get authorization token | GET | /api/Jumio/authorization-token |  |  |  |  |  |
| 2 | Jumio | Jumio callback | POST | /api/Jumio/callback |  |  |  |  |  |
| 3 | Jumio | Retrieval data | GET | /api/Jumio/retrieval-data |  |  |  |  |  |
| 4 | Jumio | Verify eKYC | POST | /api/Jumio/verify-ekyc |  |  |  |  |  |
| 5 | Jumio | Upload complete | GET | /api/Jumio/upload-complete |  |  |  |  |  |
| 6 | Jumio | Disable failed | GET | /api/Jumio/disable-failed |  |  |  |  |  |
| 7 | Jumio | Cancel or retry | GET | /api/Jumio/cancel-or-retry |  |  |  |  |  |
| 8 | Jumio | Get data from Jumio | GET | /api/Jumio/data-from-jumio/{WorkflowExecutionId} |  |  |  |  |  |
| 9 | Jumio | Sync data | GET | /api/Jumio/sync-data/{WorkflowExecutionId} |  |  |  |  |  |
| 10 | Jumio | Encrypt personal data | PUT | /api/Jumio/encrypt-perisonal-data |  |  |  |  |  |
| 11 | Singpass | Authorize API | GET | /api/singpass/authorize-api |  |  |  |  |  |
| 12 | Singpass | Retrieval data | POST | /api/singpass/retrieval-data |  |  |  |  |  |
| 13 | Singpass | Verify eKYC | POST | /api/singpass/verify-ekyc |  |  |  |  |  |
| 14 | Singpass | JWKS | GET | /api/singpass/jwks |  |  |  |  |  |
| 15 | eKYC status | Get eKYC status | GET | /api/v2.0/Customer/ekyc-status |  |  |  |  |  |

### C.4 eSIM & Data Plan (Mobile)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | eSim | Activate eSim | POST | /api/Esims/activate |  |  |  |  |  |
| 2 | eSim | Web activate eSim | POST | /api/Esims/web-activate |  |  |  |  |  |
| 3 | eSim | Add data plan | POST | /api/Esims/add-data-plan/{id} |  |  |  |  |  |
| 4 | eSim | Get eSim by userId | GET | /api/Esims/{userId} |  |  |  |  |  |
| 5 | eSim | Transfer data plan | POST | /api/Esims/transfer-data-plan |  |  |  |  |  |
| 6 | eSim | Cancel data plan (v1) | POST | /api/Esims/cancel-data-plan/{id} |  |  |  |  |  |
| 7 | eSim | Cancel data plan (v2) | POST | /api/Esims/cancel-data-plan/{userId}/{id} |  |  |  |  |  |
| 8 | eSim | Get eSim status | GET | /api/Esims/esim-status |  |  |  |  |  |
| 9 | eSim | Send active code | POST | /api/Esims/send-active-code/{userId} |  |  |  |  |  |
| 10 | eSim | Revert redeem data | POST | /api/Esims/revert-redeem-data |  |  |  |  |  |
| 11 | eSim | Check activation data plan | POST | /api/Esims/check-activation-data-plan/{id} |  |  |  |  |  |
| 12 | eSim | Extend subscription expire date | POST | /api/Esims/extend-subscription-expire-date-by-user-data-plan |  |  |  |  |  |
| 13 | Data wallet | Search wallet v2 (GET) | GET | /api/data-wallet/search/v2 |  |  |  |  |  |
| 14 | Data wallet | Search wallet v2 (POST) | POST | /api/data-wallet/search/v2 |  |  |  |  |  |
| 15 | Data wallet | Search history v2 | POST | /api/data-wallet/search-history/v2 |  |  |  |  |  |
| 16 | Data wallet | Search history v3 | POST | /api/data-wallet/search-history/v3 |  |  |  |  |  |
| 17 | Data wallet | Get wallet root | GET | /api/data-wallet |  |  |  |  |  |
| 18 | Data wallet | Search by userId v2 | POST | /api/data-wallet/search-userId/v2 |  |  |  |  |  |
| 19 | Data wallet | View by subscriptionId | GET | /api/data-wallet/view-subscriptionId/{subscriptionId} |  |  |  |  |  |
| 20 | ICCID | Get ICCID and supported countries | GET | /api/v2.0/Customer/iccid-and-supported-countries |  |  |  |  |  |
| 21 | Data | Manual transfer data | POST | /api/Data/manual-transfer |  |  |  |  |  |
| 22 | Data | Manual transfer v2 | POST | /api/Data/manual-transfer-v2 |  |  |  |  |  |
| 23 | Data | Reactive transfer data | POST | /api/Data/reactive-transfer-data |  |  |  |  |  |
| 24 | Data | Reactive eSim | POST | /api/Data/reactive-esim |  |  |  |  |  |

### C.5 Cashback & Rewards

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Cashback | Get cashback summary (v2) | GET | /api/v2.0/Customer/Cashback/Summary |  |  |  |  |  |
| 2 | Cashback | Get cashback detail | GET | /api/v2.0/Customer/Cashback/Detail |  |  |  |  |  |
| 3 | Cashback | Get cashback summary (v3) | GET | /api/v3.0/Customer/Cashback/Summary |  |  |  |  |  |
| 4 | Cashback | Get cashback activities (v3) | POST | /api/v3.0/Customer/Cashback/CashbackActivities |  |  |  |  |  |
| 5 | Cashback | Add cashback | POST | /api/v2.0/CashBack |  |  |  |  |  |
| 6 | Cashback | Correct user balance | POST | /api/v2.0/CashBack/correct-user-balance |  |  |  |  |  |
| 7 | Cashback | Send pending cashback notification | POST | /api/v2.0/CashBack/send-pending-cashback-notification |  |  |  |  |  |
| 8 | Cashback | Missing cashback | POST | /api/v2.0/CashBack/Missing |  |  |  |  |  |
| 9 | Cashback partner | Active cashback partners | GET | /api/v2.0/CashbackPartner/ActivePartners |  |  |  |  |  |
| 10 | Cashback partner | List cashback partners | POST | /api/v2.0/CashbackPartner/Partners |  |  |  |  |  |
| 11 | Cashback partner | Create cashback partner | POST | /api/v2.0/CashbackPartner/create |  |  |  |  |  |
| 12 | Cashback partner | Update cashback partner | PUT | /api/v2.0/CashbackPartner/update |  |  |  |  |  |
| 13 | Cashback partner | Get all cashback partners | GET | /api/v2.0/CashbackPartner |  |  |  |  |  |
| 14 | Cashback partner | Get cashback partner by id | GET | /api/v2.0/CashbackPartner/{idPartner} |  |  |  |  |  |
| 15 | Cashback partner | Delete cashback partner | DELETE | /api/v2.0/CashbackPartner/delete |  |  |  |  |  |

### C.6 Payment & Card (Mobile)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Card v2 | Get saved cards | GET | /api/v2.0/CardDetails |  |  |  |  |  |
| 2 | Card v2 | Add card | POST | /api/v2.0/CardDetails |  |  |  |  |  |
| 3 | Card v2 | Delete card | DELETE | /api/v2.0/CardDetails |  |  |  |  |  |
| 4 | Card v2 | Change primary card | POST | /api/v2.0/CardDetails/ChangePrimaryCreditCard |  |  |  |  |  |
| 5 | Card v2.1 | Get saved cards | GET | /api/v2.1/CardDetails |  |  |  |  |  |
| 6 | Card v2.1 | Add card | POST | /api/v2.1/CardDetails |  |  |  |  |  |
| 7 | Card v2.1 | Delete card | DELETE | /api/v2.1/CardDetails |  |  |  |  |  |
| 8 | Card v2.1 | Change primary card | POST | /api/v2.1/CardDetails/ChangePrimaryCreditCard |  |  |  |  |  |
| 9 | Card v2.1 | Setup intent | POST | /api/v2.1/CardDetails/setup-intent |  |  |  |  |  |
| 10 | Card v2.1 | Get mobile cards | GET | /api/v2.1/CardDetails/mobile |  |  |  |  |  |
| 11 | Card v2.1 | Create Stripe customer | POST | /api/v2.1/CardDetails/create-stripe-customer |  |  |  |  |  |
| 12 | PayPal | Add PayPal | POST | /api/v2.0/Paypal |  |  |  |  |  |
| 13 | PayPal | Get PayPal accounts | GET | /api/v2.0/Paypal |  |  |  |  |  |
| 14 | PayPal | Delete PayPal | DELETE | /api/v2.0/Paypal/{Id} |  |  |  |  |  |

### C.7 Inbox & Notifications (Mobile)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Inbox | Add inbox message | POST | /api/inbox/add-inbox |  |  |  |  |  |
| 2 | Inbox | Get inbox | GET | /api/inbox |  |  |  |  |  |
| 3 | Inbox | Update inbox status | POST | /api/inbox/UpdateStatus |  |  |  |  |  |
| 4 | Inbox | Get unread count | GET | /api/inbox/unread-number |  |  |  |  |  |
| 5 | Notification | Save Firebase token | POST | /api/v2.0/Notifications/SaveFirebaseToken |  |  |  |  |  |
| 6 | Notification | Get notification preference | POST | /api/v2.0/Notifications/GetNotificationUserPreference |  |  |  |  |  |
| 7 | Notification | Save notification preference | POST | /api/v2.0/Notifications/PostNotificationUserPreference |  |  |  |  |  |
| 8 | Notification | Check firebase token | GET | /api/v2.0/Notifications/check-firebase-token |  |  |  |  |  |

### C.8 Referral & Invite Friends

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Invite | Get invite friends info (GET) | GET | /api/v2.0/Customer/invite-friends-infor |  |  |  |  |  |
| 2 | Invite | Get invite friends info (POST) | POST | /api/v2.0/Customer/invite-friends-infor |  |  |  |  |  |
| 3 | Invite | Show extra invite box | GET | /api/v2.0/Customer/show-extra-invite-box |  |  |  |  |  |
| 4 | Invite | Get invite history | GET | /api/v2.0/Customer/invite-friends-history |  |  |  |  |  |
| 5 | Referral | Pending referrals | GET | /api/v2.0/Customer/pending-referrals |  |  |  |  |  |
| 6 | Affiliate | Affiliate click | POST | /api/v2.0/Customer/affiliate/click |  |  |  |  |  |
| 7 | Shopback | Create Shopback order | POST | /api/shopback |  |  |  |  |  |
| 8 | Shopback | Create shopback order (GET) | GET | /api/shopback/create-shopback-order |  |  |  |  |  |
| 9 | Shopback | Validate shopback order | GET | /api/shopback/validate-shopback-order |  |  |  |  |  |

### C.9 Customer Level Program & Milestones

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Level | Get level program | GET | /api/customer-level-program |  |  |  |  |  |
| 2 | Level | Get level by number | GET | /api/customer-level-program/level/{levelNumber} |  |  |  |  |  |
| 3 | Reward | Get reward by level | GET | /api/customer-level-program/reward/{levelId} |  |  |  |  |  |
| 4 | User history | Get user program history | GET | /api/v2.0/Customer/user-program-history |  |  |  |  |  |
| 5 | Milestone | Get level by id | GET | /api/general-level-milestone-module/level-Id |  |  |  |  |  |
| 6 | Milestone | Get level by number | GET | /api/general-level-milestone-module/level-number |  |  |  |  |  |
| 7 | Milestone | Create level | POST | /api/general-level-milestone-module/level |  |  |  |  |  |
| 8 | Milestone | Start level | POST | /api/general-level-milestone-module/level/start |  |  |  |  |  |
| 9 | Milestone | End level | POST | /api/general-level-milestone-module/level/end |  |  |  |  |  |
| 10 | Milestone | Get reward | GET | /api/general-level-milestone-module/reward |  |  |  |  |  |
| 11 | Milestone | Create reward | POST | /api/general-level-milestone-module/reward |  |  |  |  |  |
| 12 | Milestone | Get reward by type | GET | /api/general-level-milestone-module/reward/{rewardType} |  |  |  |  |  |

### C.10 BHN & Voucher Purchase (Mobile)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | BHN | Create transaction (network) | POST | /api/transactionManagement/v2/transaction/network |  |  |  |  |  |
| 2 | BHN | Create transaction | POST | /api/transactionManagement/v2/transaction |  |  |  |  |  |
| 3 | BHN | Reverse transaction | POST | /api/transactionManagement/v2/transaction/reverse |  |  |  |  |  |
| 4 | Voucher | Get voucher/gift/promo info (GET) | GET | /api/v2.0/Customer/voucher-gift-promo-infor |  |  |  |  |  |
| 5 | Voucher | Get voucher/gift/promo info (POST) | POST | /api/v2.0/Customer/voucher-gift-promo-infor |  |  |  |  |  |
| 6 | Promo | Get home promo info | GET | /api/v2.0/Customer/home-promo-info |  |  |  |  |  |
| 7 | Gift code | Apply promo code | POST | /api/gift-code/promo/apply |  |  |  |  |  |
| 8 | Gift code | Get promo codes | GET | /api/gift-code/promo/get |  |  |  |  |  |
| 9 | Gift code | Get promo short content | GET | /api/gift-code/promo/short-content |  |  |  |  |  |
| 10 | Gift code | Sign-up gift code | GET | /api/gift-code/sign-up-gift-code |  |  |  |  |  |
| 11 | Gift code | Check sign-up gift code | POST | /api/gift-code/check-sign-up-gift-code |  |  |  |  |  |

### C.11 AI Agents (Mobile)

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Agent | Initiate customer sign-up | POST | /api/agents/customer-sign-up/initiate |  |  |  |  |  |
| 2 | Agent | Consume customer sign-up | POST | /api/agents/customer-sign-up/consume |  |  |  |  |  |
| 3 | Agent | Check phone number | POST | /api/agents/customer/check-phone |  |  |  |  |  |
| 4 | Agent | Redeem gift code | POST | /api/agents/customer/redeem-gift-code |  |  |  |  |  |
| 5 | Agent | Assign eSim | POST | /api/agents/customer/assign-esim |  |  |  |  |  |

### C.12 QR Codes & Dynamic Links

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | QR | Generate QR code | GET | /api/QRCodes/generator |  |  |  |  |  |
| 2 | QR | Generate QR base64 | GET | /api/QRCodes/generator/base64 |  |  |  |  |  |
| 3 | QR | Send QR by mail | POST | /api/QRCodes/send-mail |  |  |  |  |  |
| 4 | QR | URL generator | GET | /api/QRCodes/urlgenerator |  |  |  |  |  |
| 5 | QR | Aircode generator | GET | /api/QRCodes/aircode-generator |  |  |  |  |  |
| 6 | Link | Dynamic link (referral) | POST | /api/Link |  |  |  |  |  |
| 7 | Link | Refresh shared-gifts fallback | POST | /api/dynamic-links/shared-gifts/refresh-fallback |  |  |  |  |  |
| 8 | Image | Get image via viewer | GET | /api/image-viewer/get-image |  |  |  |  |  |

---

## D. EXTERNAL PARTNER API

> API tích hợp cho bên thứ ba (partner dùng API key để tích hợp trực tiếp).

### D.1 Gift Code API

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Gift code | Search gift codes | POST | /api/external-api-adapter/gift-code/search |  |  |  |  |  |
| 2 | Gift code | Delete single gift code | DELETE | /api/external-api-adapter/gift-code/delete/{giftCode} |  |  |  |  |  |
| 3 | Gift code | Delete multiple gift codes | DELETE | /api/external-api-adapter/gift-code/multi-delete |  |  |  |  |  |
| 4 | Gift code | Generate unique code | POST | /api/external-api-adapter/gift-code/generate-unique-code |  |  |  |  |  |
| 5 | Gift code | Export gift codes | POST | /api/external-api-adapter/gift-code/export |  |  |  |  |  |
| 6 | Gift code | Export by id | POST | /api/external-api-adapter/gift-code/export-by-id |  |  |  |  |  |
| 7 | Gift code | List campaigns | POST | /api/external-api-adapter/gift-code/list-campaign |  |  |  |  |  |

### D.2 Voucher API

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Voucher | Refund voucher | POST | /api/external-api-adapter/voucher/refund |  |  |  |  |  |
| 2 | Voucher | Search vouchers | POST | /api/external-api-adapter/voucher/search |  |  |  |  |  |
| 3 | Voucher | Purchase voucher | POST | /api/external-api-adapter/voucher/purchase |  |  |  |  |  |
| 4 | Voucher | Export vouchers | POST | /api/external-api-adapter/voucher/export |  |  |  |  |  |
| 5 | Voucher | Export by IDs | POST | /api/external-api-adapter/voucher/export-by-Ids |  |  |  |  |  |
| 6 | Voucher | Data purchase | POST | /api/external-api-adapter/voucher/data-purchase |  |  |  |  |  |

### D.3 Data Plan API

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Data plan | Search data plans | POST | /api/external-api-adapter/data-plan/search |  |  |  |  |  |
| 2 | Data plan | Get all data plans | GET | /api/external-api-adapter/all-data-plan |  |  |  |  |  |
| 3 | Partner | Get partner info | GET | /api/external-api-adapter/partner |  |  |  |  |  |
| 4 | Partner | Get partner credit | GET | /api/external-api-adapter/partner-credit |  |  |  |  |  |

### D.4 Translation Queue API

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Translate | Trigger translate queue | POST | /api/external-api-adapter/translate-queue/trigger |  |  |  |  |  |
| 2 | Translate | Get translate queue by id | GET | /api/external-api-adapter/translate-queue/{id} |  |  |  |  |  |
| 3 | Translate | Retry trigger | POST | /api/external-api-adapter/translate-queue/retry-trigger |  |  |  |  |  |

### D.5 Permissions API

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Permission | Get user permissions | GET | /api/external-api-adapter/account-permision/user-permision |  |  |  |  |  |
| 2 | Permission | Get role permissions | GET | /api/external-api-adapter/account-permision/role-permision |  |  |  |  |  |

### D.6 Worker Management API

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | User | Create external user | POST | /api/external-api-adapter/users |  |  |  |  |  |
| 2 | User | Update external user | PUT | /api/external-api-adapter/users |  |  |  |  |  |
| 3 | User | Search external users | POST | /api/external-api-adapter/users/search |  |  |  |  |  |
| 4 | User | Delete external user | DELETE | /api/external-api-adapter/users/{accountId} |  |  |  |  |  |
| 5 | eSim | External top-up | POST | /api/external-api-adapter/esim/top-up |  |  |  |  |  |
| 6 | eSim | External data wallet | POST | /api/external-api-adapter/esim/data-wallet |  |  |  |  |  |

---

## E. SYSTEM / WEBHOOK / INTERNAL

> Các endpoint kỹ thuật, webhook bên thứ 3, và tiện ích hệ thống nội bộ.

### E.1 Health Check & Validators

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Health | Health check | GET | /api/health-check |  |  |  |  |  |
| 2 | Validator | Data wallet validator | GET | /api/Validators/data-wallet |  |  |  |  |  |

### E.2 SingTel Hub & Provisioning

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Hub | SingTel hub webhook | POST | /api/Hubs/singtel |  |  |  |  |  |
| 2 | SingTels | SingTel create/update | POST | /api/SingTels |  |  |  |  |  |
| 3 | SingTels | SingTel provisioning | POST | /api/SingTels/provisioning |  |  |  |  |  |
| 4 | eSim ops | Suspend eSim | POST | /api/Esims/suspend-esim |  |  |  |  |  |
| 5 | eSim ops | Change eSim status | PUT | /api/Esims/change-esim-status |  |  |  |  |  |
| 6 | eSim ops | Sync up eSim status | PUT | /api/Esims/sync-up-esim-status |  |  |  |  |  |
| 7 | eSim ops | Sync up list eSim status | PUT | /api/Esims/sync-up-list-esim-status |  |  |  |  |  |
| 8 | eSim ops | Cancel SingTel data plan | POST | /api/Esims/cancel-singtel-data-plan |  |  |  |  |  |
| 9 | eSim ops | Mapping SingTel data | GET | /api/Esims/mapping-singtel-data |  |  |  |  |  |
| 10 | eSim ops | Account duplicate SingTel data | GET | /api/Esims/account-duplicate-singtel-data |  |  |  |  |  |
| 11 | eSim ops | Handle out-of-stock job | POST | /api/Esims/handle-out-of-stock-esim-job |  |  |  |  |  |
| 12 | eSim ops | Validator eSim status | PUT | /api/Esims/validator-esim-status |  |  |  |  |  |
| 13 | eSim ops | Set configure lock query subscriber | POST | /api/Esims/set-configure-lock-query-subscriber |  |  |  |  |  |
| 14 | eSim ops | Retry update KYC | POST | /api/Esims/re-try-update-kyc |  |  |  |  |  |

### E.3 Shopee Integration

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Auth | Get authorization link | GET | /api/shopee/authorization-link |  |  |  |  |  |
| 2 | Auth | Get token | GET | /api/shopee/token |  |  |  |  |  |
| 3 | Auth | Get access token | GET | /api/shopee/access-token |  |  |  |  |  |
| 4 | Order | Get order details | GET | /api/shopee/order-details |  |  |  |  |  |
| 5 | Order | Create voucher by order id | POST | /api/shopee/create-voucher-by-order-id |  |  |  |  |  |
| 6 | Order | Update ship order | POST | /api/shopee/update-ship-order |  |  |  |  |  |
| 7 | Order | Update order to shipped | POST | /api/shopee/update-order-to-shipped |  |  |  |  |  |
| 8 | Invoice | Buyer invoice | POST | /api/shopee/buyer-invoice |  |  |  |  |  |
| 9 | Shipping | Get shipping parameter | GET | /api/shopee/shipping-parameter |  |  |  |  |  |
| 10 | Finance | Get escrow detail | GET | /api/shopee/escrow-detail |  |  |  |  |  |
| 11 | Fix | Correct data | GET | /api/shopee/correct-data |  |  |  |  |  |
| 12 | Webhook | Order webhook (GET) | GET | /api/shopee-push-notifications/order-webhook |  |  |  |  |  |
| 13 | Webhook | Order webhook (POST) | POST | /api/shopee-push-notifications/order-webhook |  |  |  |  |  |

### E.4 Shopback Reports

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Shopback | Search | POST | /api/shopback/Search |  |  |  |  |  |
| 2 | Shopback | Export | POST | /api/shopback/Export |  |  |  |  |  |
| 3 | Shopback | Import payment | POST | /api/shopback/payment/import |  |  |  |  |  |
| 4 | Shopback | Get import setting | GET | /api/shopback/import-setting |  |  |  |  |  |
| 5 | Shopback | Save import setting | POST | /api/shopback/import-setting |  |  |  |  |  |
| 6 | Shopback | Get import setting by id | GET | /api/shopback/import-setting/{Id} |  |  |  |  |  |
| 7 | Shopback | Confirm import payment | POST | /api/shopback/payment/confirm-import |  |  |  |  |  |
| 8 | Shopback | Search payment | POST | /api/shopback/payment/search |  |  |  |  |  |
| 9 | Shopback | Update payment status | POST | /api/shopback/payment/status |  |  |  |  |  |
| 10 | Shopback | Get payment id list | GET | /api/shopback/payment/payment-id-list |  |  |  |  |  |
| 11 | Shopback | Check payment | GET | /api/shopback/payment/check |  |  |  |  |  |
| 12 | Shopback | Import commissions | POST | /api/shopback/import |  |  |  |  |  |
| 13 | Shopback | Confirm import commissions | POST | /api/shopback/confirm-import |  |  |  |  |  |

### E.5 Sample & Debug Endpoints

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Sample | Post sample v1 | POST | /api/Sample/Post-sample |  |  |  |  |  |
| 2 | Sample | Post sample v2 | POST | /api/Sample/Post-sample-2 |  |  |  |  |  |
| 3 | Sample | Simulate 500 error | POST | /api/Sample/500-internal-server-error |  |  |  |  |  |
| 4 | Sample | Get SingTel host | GET | /api/Sample/singtel-host |  |  |  |  |  |
| 5 | Sample | Change subscription status | POST | /api/Sample/change-subscription-status |  |  |  |  |  |

### E.6 Captive Page & Misc Utilities

| # | Function | Action | Method | Endpoint | UI | Web | API | QC | Note |
|---|----------|--------|--------|----------|----|----|----|-----|------|
| 1 | Captive page | Search captive pages | POST | /api/captive-page-management/Search |  |  |  |  |  |
| 2 | Captive page | Export captive pages | POST | /api/captive-page-management/Export |  |  |  |  |  |
| 3 | Image | View image | GET | /api/image-viewer/get-image |  |  |  |  |  |
| 4 | Click tracking | Clicks (POST) | POST | /api/v2.0/Customer/Clicks |  |  |  |  |  |
| 5 | Click tracking | Clicks (GET) | GET | /api/v2.0/Customer/Clicks |  |  |  |  |  |
| 6 | Click tracking | Clicks by customer | POST | /api/v2.0/Customer/Clicks-By-Customer |  |  |  |  |  |
| 7 | Click tracking | Tracked clicks by customer | POST | /api/v2.0/Customer/Tracked-Clicks-By-Customer |  |  |  |  |  |
| 8 | Click events | Get click events | GET | /api/v2.0/Customer/ClickEvents |  |  |  |  |  |
| 9 | Click events | Update click events | PUT | /api/v2.0/Customer/ClickEvents |  |  |  |  |  |
| 10 | Click orders | Get click orders | GET | /api/v2.0/Customer/ClickOrders |  |  |  |  |  |
| 11 | Click | Customer click search | POST | /api/v2.0/Customer/customer-click/search |  |  |  |  |  |
| 12 | Click | Customer click export | POST | /api/v2.0/Customer/customer-click/export |  |  |  |  |  |

---

*WBS hoàn chỉnh — tổng cộng toàn bộ endpoint từ swagger.json (Eskimo API v1)*
