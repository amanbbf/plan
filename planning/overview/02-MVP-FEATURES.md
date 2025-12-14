# MVP Features Breakdown

## Feature Priority Matrix

### P0 - Must Have (Launch Blockers)
Without these, the platform cannot function.

| # | Feature | Admin | Buyer |
|---|---------|-------|-------|
| 1 | User Authentication (JWT) | ✅ | ✅ |
| 2 | Buyer Registration with GST | - | ✅ |
| 3 | Buyer Approval Workflow | ✅ | - |
| 4 | Category Management | ✅ | - |
| 5 | Product Management (CRUD) | ✅ | - |
| 6 | Product Catalog View | - | ✅ |
| 7 | Search & Filter Products | - | ✅ |
| 8 | MOQ Enforcement | ✅ | ✅ |
| 9 | Cart Functionality | - | ✅ |
| 10 | Checkout Flow | - | ✅ |
| 11 | Order Creation | - | ✅ |
| 12 | Order Management | ✅ | - |
| 13 | Order Status Updates | ✅ | ✅ |
| 14 | GST Invoice Generation | ✅ | ✅ |
| 15 | Pin-code Serviceability | ✅ | ✅ |
| 16 | Shipping Assignment | ✅ | - |
| 17 | Order Tracking View | - | ✅ |

---

### P1 - Should Have (Important for UX)
Platform works without these, but user experience suffers.

| # | Feature | Admin | Buyer |
|---|---------|-------|-------|
| 1 | Dashboard Analytics | ✅ | - |
| 2 | UPI Payment Integration | ✅ | ✅ |
| 3 | COD Option | ✅ | ✅ |
| 4 | Credit Limit Management | ✅ | ✅ |
| 5 | Bulk Discounts | ✅ | ✅ |
| 6 | Multiple Addresses | - | ✅ |
| 7 | Order History | ✅ | ✅ |
| 8 | Invoice Download | ✅ | ✅ |
| 9 | Push Notifications | ✅ | ✅ |
| 10 | Stock Management | ✅ | - |

---

### P2 - Nice to Have (Post-MVP)
Can be added after successful launch.

| # | Feature | Notes |
|---|---------|-------|
| 1 | Bulk Product Upload (CSV) | Admin efficiency |
| 2 | Reports Export | Finance needs |
| 3 | SMS Notifications | Customer communication |
| 4 | Wishlist/Favorites | Buyer convenience |
| 5 | Repeat Order | Buyer convenience |
| 6 | Advanced Analytics | Business insights |
| 7 | Staff Role Management | Team scaling |
| 8 | Audit Logs | Compliance |

---

## Feature Details

### 1. Authentication System

**Admin Login**
- Email + Password
- Role-based access
- Session management
- Password reset

**Buyer Registration**
- Phone + OTP verification
- Business details
- GST number (optional but validated)
- Document upload (future)
- Admin approval required

**Buyer Login**
- Phone + OTP
- Remember device option
- Auto-logout on inactivity

---

### 2. Product Management

**Product Fields**
- Name
- Description
- SKU
- HSN Code
- Category / Sub-category
- Images (multiple)
- MRP
- Selling Price
- GST Rate (5%, 12%, 18%, 28%)
- MOQ (Minimum Order Quantity)
- Stock quantity
- Unit (kg, piece, box, dozen, etc.)
- Status (active/inactive)

**Category Structure**
```
Category (e.g., Beverages)
  └── Sub-category (e.g., Cold Drinks)
       └── Products (e.g., Coca Cola 2L)
```

---

### 3. Order Lifecycle

```
[PENDING] → Order placed, awaiting processing
    ↓
[CONFIRMED] → Admin confirmed, preparing
    ↓
[PACKED] → Items packed, ready for dispatch
    ↓
[SHIPPED] → Handed to courier, AWB assigned
    ↓
[IN_TRANSIT] → On the way (optional status)
    ↓
[DELIVERED] → Successfully delivered
    ↓
[COMPLETED] → Payment received, order closed

Alternative flows:
[PENDING] → [CANCELLED] (by buyer or admin)
[SHIPPED] → [RETURNED] (if delivery fails)
```

---

### 4. Invoice System

**Invoice Contains**
- Invoice number (auto-generated)
- Invoice date
- Seller details (company name, GST, address)
- Buyer details (business name, GST, address)
- Product line items
  - Product name
  - HSN code
  - Quantity
  - Unit price
  - GST rate
  - Tax amount
  - Total
- Sub-total
- CGST / SGST / IGST breakup
- Grand total
- Payment terms
- Notes

**Invoice Format**
- PDF generation
- Print-ready
- Downloadable

---

### 5. Payment System

**Payment Methods**
| Method | Flow |
|--------|------|
| UPI | Online payment → Auto-confirmation |
| COD | Mark as COD → Collect on delivery → Admin confirms |
| Credit | Admin assigns credit limit → Buyer uses credit → Manual settlement |

**Payment Status**
- Pending
- Paid
- Partial
- Failed
- Refunded

---

### 6. Serviceability System

**Pin-code Management**
- Admin adds serviceable pin codes
- Estimated delivery days per pin code
- Delivery charges per pin code (optional)
- Zone grouping (optional)

**Serviceability Check**
- Buyer enters delivery pin code
- System checks if serviceable
- Shows estimated delivery time
- Shows delivery charges (if any)

---

### 7. Credit System (Simple)

**Credit Ledger**
- Credit limit assigned by admin
- Available credit = Limit - Outstanding
- Order reduces available credit
- Payment increases available credit

**Ledger Entries**
- Order placed (debit)
- Payment received (credit)
- Adjustment (admin manual)

---

### 8. Notification System

**Notification Types**
| Event | Buyer | Admin |
|-------|-------|-------|
| New registration | - | ✅ |
| Account approved | ✅ | - |
| Order placed | ✅ | ✅ |
| Order status change | ✅ | - |
| Payment received | ✅ | ✅ |
| Credit limit updated | ✅ | - |

**Channels (MVP)**
- In-app notifications
- Push notifications (mobile)

**Future Channels**
- SMS
- Email
- WhatsApp

---

## User Flow Diagrams

### Buyer Journey
```
1. Download App / Visit Website
2. Register with phone + business details
3. Wait for admin approval
4. Login after approval
5. Browse products by category
6. Search for specific products
7. Add items to cart (MOQ enforced)
8. Proceed to checkout
9. Select/add delivery address
10. Check serviceability (pin code)
11. Choose payment method
12. Place order
13. Receive order confirmation
14. Track order status
15. Receive shipment
16. Download invoice
```

### Admin Order Processing
```
1. Login to admin panel
2. View new orders on dashboard
3. Click on order to view details
4. Verify payment status
5. Confirm order
6. Pack items
7. Update status to "Packed"
8. Choose courier service
9. Enter AWB/tracking number
10. Update status to "Shipped"
11. Generate invoice
12. Monitor delivery status
13. Mark as delivered
14. Close order
```

---

## Data Requirements

### Master Data
- Categories & Sub-categories
- Products with pricing
- Serviceable pin codes
- GST rates
- Payment methods
- Order statuses

### Transactional Data
- Buyer registrations
- Orders
- Invoices
- Payments
- Credit ledger entries
- Notifications

### Reporting Data
- Daily/Weekly/Monthly sales
- Top products
- Top buyers
- Payment collection
- Outstanding credits

---

## Integration Points

### MVP Integrations
| Integration | Purpose | Priority |
|-------------|---------|----------|
| OTP Service | Phone verification | P0 |
| PDF Generator | Invoice creation | P0 |
| Cloud Storage | Image uploads | P0 |
| UPI Gateway | Online payments | P1 |

### Future Integrations
| Integration | Purpose |
|-------------|---------|
| SMS Gateway | Notifications |
| Email Service | Invoices, notifications |
| Shipping APIs | Auto tracking |
| WhatsApp Business | Order updates |
| Accounting Software | Tally export |
