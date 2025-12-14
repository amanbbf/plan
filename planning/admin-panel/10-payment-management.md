# Admin Panel - Payment Management Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Payment Management |
| URL | `/admin/payments` |
| Access | Super Admin, Finance |
| Priority | P1 - Important |
| Responsive | Desktop only |

---

## Purpose
Track all payments, manage payment status, handle manual payment recording, and manage credit/outstanding balances.

---

## UI Components

### 1. Page Header
- Title: "Payments"
- Date range filter
- Search (order ID, transaction ID)
- "Record Payment" button
- Export button

### 2. Quick Stats
| Stat | Description |
|------|-------------|
| Today's Collection | Amount collected today |
| This Month | Total this month |
| Pending Payments | Unpaid order amount |
| Outstanding Credit | Total credit outstanding |

### 3. Tabs
- **All Payments** - Complete history
- **UPI** - Online payments
- **COD** - Cash on delivery
- **Credit** - Credit payments
- **Pending** - Awaiting payment

### 4. Payments Table
| Column | Description | Sortable |
|--------|-------------|----------|
| Date | Payment date | Yes |
| Order ID | Linked order | Yes |
| Buyer | Business name | Yes |
| Amount | ₹ paid | Yes |
| Method | UPI/COD/Credit | Yes |
| Status | Success/Pending/Failed | Yes |
| Transaction ID | Reference number | No |
| Actions | View, Update | No |

---

## Payment Details Modal

### Payment Information
| Field | Value |
|-------|-------|
| Payment ID | Unique identifier |
| Order ID | Linked order |
| Buyer | Business name |
| Method | Payment method |
| Amount | ₹ amount |
| Status | Current status |
| Transaction ID | Reference |
| Date/Time | When paid |
| Recorded By | Admin (if manual) |

### Actions Available
- Mark as Paid (if pending COD/Credit)
- Mark as Failed
- Add Note
- View Order

---

## Record Manual Payment

For COD and Credit payments:

### Form Fields
| Field | Type | Required |
|-------|------|----------|
| Order/Buyer | Search dropdown | Yes |
| Amount | Number | Yes |
| Payment Method | Dropdown | Yes |
| Transaction Reference | Text | No |
| Payment Date | Date picker | Yes |
| Notes | Text area | No |
| Attachment | File upload (receipt) | No |

---

## Credit Management Section

### Credit Overview Table
| Column | Description |
|--------|-------------|
| Buyer | Business name |
| Credit Limit | Assigned limit |
| Total Outstanding | Current dues |
| Available Credit | Remaining |
| Last Payment | Date & amount |
| Overdue | Days overdue (if any) |

### Buyer Credit Details
When clicking on a buyer:
- Credit limit
- Credit ledger entries
- Outstanding orders
- Payment history
- Age analysis (30/60/90 days)

### Credit Actions
- Update credit limit
- Record payment against credit
- Block credit (if overdue)
- Send payment reminder

---

## Credit Ledger Entry

```json
{
  "_id": "credit_123",
  "buyer": "buyer_123",
  "type": "debit",  // or "credit"
  "amount": 5000,
  "balance": 15000,
  "reference": {
    "type": "order",  // or "payment"
    "id": "order_123"
  },
  "description": "Order #ORD-2024-001",
  "date": "2024-01-15",
  "recordedBy": "admin_123"
}
```

---

## API Calls

### GET `/api/admin/payments`
List all payments with filters.

### GET `/api/admin/payments/:id`
Get payment details.

### POST `/api/admin/payments`
Record manual payment.
```json
{
  "orderId": "order_123",
  "amount": 5000,
  "method": "cod",
  "transactionId": "CASH-001",
  "paymentDate": "2024-01-15",
  "notes": "Collected by delivery partner"
}
```

### PUT `/api/admin/payments/:id/status`
Update payment status.

### Credit APIs

### GET `/api/admin/credit/buyers`
List buyers with credit.

### GET `/api/admin/credit/buyers/:id/ledger`
Get credit ledger for buyer.

### PUT `/api/admin/credit/buyers/:id/limit`
Update credit limit.

### POST `/api/admin/credit/buyers/:id/payment`
Record credit payment.

### GET `/api/admin/payments/stats`
Payment statistics.

---

## Payment Statuses

| Status | Description | Color |
|--------|-------------|-------|
| pending | Awaiting payment | Yellow |
| processing | Being processed (UPI) | Blue |
| success | Payment received | Green |
| failed | Payment failed | Red |
| refunded | Refund processed | Gray |

---

## Payment Methods

### UPI
- Auto-confirmed via payment gateway
- Transaction ID from gateway
- Instant confirmation

### COD (Cash on Delivery)
- Initially marked pending
- Admin marks paid on delivery confirmation
- Manual transaction reference

### Credit
- Uses buyer's credit limit
- Creates ledger entry
- Payment collected later
- Can be partial payments

---

## Age Analysis (Credit)

### Outstanding Buckets
| Bucket | Days | Amount |
|--------|------|--------|
| Current | 0-30 | ₹50,000 |
| 30 Days | 31-60 | ₹25,000 |
| 60 Days | 61-90 | ₹10,000 |
| 90+ Days | >90 | ₹5,000 |

### Actions for Overdue
- Send reminder
- Block further credit orders
- Escalate to management

---

## Reports

### Collection Report
- Daily/Weekly/Monthly collections
- By payment method
- By buyer
- Trend analysis

### Outstanding Report
- Buyer-wise outstanding
- Age-wise analysis
- Overdue alerts

### Reconciliation Report
- UPI transactions vs orders
- COD collection vs deliveries
- Discrepancy flagging

---

## Checklist

### Development
- [ ] Create payments list page
- [ ] Implement payment tabs
- [ ] Build payment details modal
- [ ] Create manual payment recording
- [ ] Build credit management section
- [ ] Implement credit ledger
- [ ] Add credit limit management
- [ ] Create age analysis
- [ ] Build payment reports
- [ ] Add export functionality

### Testing
- [ ] Payment list displays correctly
- [ ] Filters and tabs work
- [ ] Manual payment recording works
- [ ] Credit ledger updates correctly
- [ ] Credit limit enforcement works
- [ ] Outstanding calculates correctly
- [ ] Reports generate accurately

---

## Dependencies
- Order management system
- Buyer management system
- Report generation utility
- Export utility
- Payment gateway integration (for UPI)
