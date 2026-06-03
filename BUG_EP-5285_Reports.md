# 🐛 Bug Reports for EP-5285

---

## BUG-001 (EP-5700)

**Summary:** `[STG - COMMISSION PAYMENTS] Warning popup incorrectly displays instead of Confirm popup when paying a valid record`

**Description:**
When an Admin selects a valid record (e.g., status is "Upcoming" or "In Progress") and clicks the "Pay" button on the Commission Payments page, a warning popup appears stating the status is invalid, instead of the expected confirmation popup.

**Steps to Reproduce:**
1. Log in to the Admin Portal.
2. Navigate to the **Commission Payments** module.
3. Select a valid record (e.g., Partner: COCCO, Status: In Progress).
4. Click the **Pay** button.
5. Observe the popup displayed.

**Test Data:**
- Record ID: `d531b91f-59d8-4396-ac1b-85d594847eea`
- Partner: `COCCO`

**Actual Result:**
A Warning popup is displayed with the message: *"You can only make payments for Partners with status 'Upcoming' or 'Insufficient'. Partners with invalid status cannot be processed for payment."*

**Expected Result:**
A Confirmation popup should be displayed showing the payment details (total amount, number of records selected) to proceed with the payment.

---

## BUG-002 (EP-5701)

**Summary:** `[STG - PARTNERSHIP] Total Balance incorrectly resets to $0 when a payment is In Progress`

**Description:**
After initiating a payment on the Commission Payments page, the record's status changes to "In Progress". However, when checking the Partnership Detail page, the Total Balance is incorrectly shown as $0 instead of the remaining balance or the previous balance.

**Steps to Reproduce:**
1. Log in to the Admin Portal.
2. Navigate to the **Commission Payments** module.
3. Initiate a payment for a Partner (e.g., Partner: COCCO, Amount: $51.3).
4. Verify the payment record status is "In Progress".
5. Navigate to the **Partnership** module.
6. Open the **Partner Detail** page for the respective partner (COCCO).
7. Check the **Total Balance (Link-based & Network Affiliate)** value at the bottom left.

**Test Data:**
- Partner: `COCCO`
- Payment amount: `$51.3` (Status: In Progress)

**Actual Result:**
The Total Balance is displayed as $0 while the payment is still "In Progress".

**Expected Result:**
The Total Balance should display the correct calculated amount (e.g. Total Earned - Payment). It should not automatically reset to $0 unless the exact total balance was paid out.
