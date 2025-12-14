# Admin Panel - Order Management Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Order Management |
| URL | `/admin/orders` |
| Access | Super Admin, Operations |
| Priority | P0 - Critical |
| Responsive | Desktop only |

---

## Purpose
Central hub for viewing, processing, and managing all orders through their complete lifecycle - from order placement to delivery confirmation.

---

## Order Lifecycle

```
[PENDING] → Order placed, awaiting admin action
    ↓
[CONFIRMED] → Admin confirmed, preparing order
    ↓
[PACKED] → Items packed, ready for dispatch
    ↓
[SHIPPED] → Handed to courier, tracking assigned
    ↓
[IN_TRANSIT] → On the way (optional)
    ↓
[DELIVERED] → Successfully delivered
    ↓
[COMPLETED] → Payment confirmed, order closed

Alternative Statuses:
[CANCELLED] → Cancelled by buyer or admin
[RETURNED] → Delivery failed / returned
[REFUNDED] → Refund processed
```

---

## UI Components

### 1. Page Header
- Title: "Order Management"
- Search bar (order ID, buyer name, phone)
- Date range picker
- "Export" button

### 2. Status Tabs
| Tab | Filter |
|-----|--------|
| All Orders | No filter |
| Pending | Status = pending |
| Processing | Status = confirmed, packed |
| Shipped | Status = shipped, in_transit |
| Delivered | Status = delivered |
| Cancelled | Status = cancelled, returned |

Each tab shows count badge.

### 3. Filter Bar
| Filter | Options |
|--------|---------|
| Date Range | Today, Last 7 days, Last 30 days, Custom |
| Payment Status | All, Paid, Unpaid, Partial |
| Payment Method | All, UPI, COD, Credit |
| Amount Range | Min - Max |

### 4. Orders Table
| Column | Description | Sortable |
|--------|-------------|----------|
| Order ID | Unique order number | Yes |
| Date | Order date/time | Yes |
| Buyer | Business name | Yes |
| Items | Item count | No |
| Amount | Order total | Yes |
| Payment | Method + status | No |
| Status | Current status badge | Yes |
| Actions | View, Update, Invoice | No |

### 5. Quick Actions (on each row)
- View Details
- Update Status
- Download Invoice
- Print Packing Slip

---

## Order Details View

### Header Section
- Order ID with copy button
- Order date & time
- Current status with timeline

### Buyer Information Card
| Field | Value |
|-------|-------|
| Business Name | Shop/Company |
| Owner Name | Contact person |
| Phone | Primary number |
| GST Number | GSTIN |

### Delivery Address Card
- Full address with pin code
- Delivery contact name
- Delivery phone number

### Order Items Table
| Column | Value |
|--------|-------|
| Product | Image + Name |
| SKU | Product code |
| Price | Unit price |
| Qty | Quantity ordered |
| Tax | GST amount |
| Total | Line total |

**Footer:**
- Subtotal
- GST breakdown (CGST/SGST or IGST)
- Delivery charges (if any)
- Discount (if any)
- **Grand Total**

### Payment Information Card
| Field | Value |
|-------|-------|
| Method | UPI/COD/Credit |
| Status | Paid/Unpaid/Partial |
| Amount Paid | ₹ amount |
| Balance Due | ₹ amount |
| Transaction ID | (for UPI) |

### Shipping Information Card
| Field | Value |
|-------|-------|
| Courier | India Post / Private courier |
| AWB Number | Tracking number |
| Shipped Date | When handed over |
| Estimated Delivery | Expected date |
| Tracking Link | External link |

### Order Timeline
Visual timeline showing:
- Order placed → [date/time]
- Confirmed → [date/time] by [admin]
- Packed → [date/time] by [admin]
- Shipped → [date/time] - AWB: XXX
- Delivered → [date/time]

### Actions Panel
- Update Status (dropdown)
- Add Tracking (input fields)
- Mark as Paid (button)
- Generate Invoice (button)
- Cancel Order (with reason)
- Add Note (internal)

---

## Status Update Flow

### Update Status Modal
1. Select new status from dropdown
2. Status-specific fields appear:

**For CONFIRMED:**
- Estimated packing date

**For PACKED:**
- Packing slip number (auto-generated)

**For SHIPPED:**
- Courier name (dropdown)
- AWB/Tracking number (required)
- Shipping date
- Estimated delivery date

**For DELIVERED:**
- Delivery date
- Received by (name)
- Proof of delivery (optional image)

**For CANCELLED:**
- Cancellation reason (required)
- Refund decision (if paid)

3. Add optional note
4. Confirm update

---

## Invoice Generation

### When Generated
- Auto-generated when order status = CONFIRMED
- Can be regenerated anytime
- Always reflects current order state

### Invoice Content
- Invoice number (auto-generated, sequential)
- Company details (seller)
- Buyer details with GST
- Product line items with HSN
- Tax breakup
- Payment terms
- Digital signature (optional)

### Invoice Actions
- View (PDF preview)
- Download (PDF)
- Print
- Email to buyer

---

## API Calls

### GET `/api/admin/orders`
**Query Params:**
- `page`, `limit`
- `status` - Filter by status
- `paymentStatus` - paid/unpaid/partial
- `paymentMethod` - upi/cod/credit
- `startDate`, `endDate`
- `search` - Order ID, buyer name
- `sortBy`, `sortOrder`

### GET `/api/admin/orders/:id`
Full order details with all related data.

### PUT `/api/admin/orders/:id/status`
Update order status.
```json
{
  "status": "shipped",
  "courier": "India Post",
  "awbNumber": "EE123456789IN",
  "shippedDate": "2024-01-15",
  "estimatedDelivery": "2024-01-20",
  "note": "Shipped via speed post"
}
```

### PUT `/api/admin/orders/:id/payment`
Update payment status.
```json
{
  "paymentStatus": "paid",
  "amountPaid": 5000,
  "paymentDate": "2024-01-15",
  "transactionId": "TXN123"
}
```

### POST `/api/admin/orders/:id/invoice`
Generate/regenerate invoice.

### GET `/api/admin/orders/:id/invoice`
Get invoice PDF.

### PUT `/api/admin/orders/:id/cancel`
Cancel order with reason.

### GET `/api/admin/orders/stats`
Order statistics for dashboard.

---

## Order Data Structure

```json
{
  "_id": "order_123",
  "orderNumber": "ORD-2024-0001",
  "buyer": {
    "_id": "buyer_123",
    "businessName": "Krishna Stores",
    "ownerName": "Ramesh Kumar",
    "phone": "9876543210",
    "gstNumber": "29AABCU9603R1ZM"
  },
  "items": [
    {
      "product": "prod_123",
      "name": "Parle-G 800g",
      "sku": "BIS-PG-800",
      "hsnCode": "1905",
      "price": 42,
      "quantity": 24,
      "gstRate": 5,
      "gstAmount": 48,
      "total": 1056
    }
  ],
  "shippingAddress": {
    "name": "Krishna Stores",
    "phone": "9876543210",
    "addressLine1": "123, Main Road",
    "addressLine2": "Near Bus Stand",
    "city": "Bangalore",
    "state": "Karnataka",
    "pincode": "560001"
  },
  "pricing": {
    "subtotal": 5000,
    "cgst": 125,
    "sgst": 125,
    "igst": 0,
    "deliveryCharge": 0,
    "discount": 100,
    "total": 5150
  },
  "payment": {
    "method": "upi",
    "status": "paid",
    "amountPaid": 5150,
    "transactionId": "TXN123",
    "paidAt": "2024-01-15T10:30:00Z"
  },
  "shipping": {
    "courier": "India Post",
    "awbNumber": "EE123456789IN",
    "shippedAt": "2024-01-16T14:00:00Z",
    "estimatedDelivery": "2024-01-20",
    "deliveredAt": null
  },
  "status": "shipped",
  "timeline": [
    { "status": "pending", "at": "...", "by": null },
    { "status": "confirmed", "at": "...", "by": "admin_123" },
    { "status": "packed", "at": "...", "by": "admin_123" },
    { "status": "shipped", "at": "...", "by": "admin_123", "note": "Speed post" }
  ],
  "invoice": {
    "number": "INV-2024-0001",
    "generatedAt": "2024-01-15T11:00:00Z",
    "url": "..."
  },
  "notes": [
    { "text": "Buyer requested early delivery", "by": "admin_123", "at": "..." }
  ],
  "createdAt": "2024-01-15T10:00:00Z",
  "updatedAt": "2024-01-16T14:00:00Z"
}
```

---

## Business Rules

### Status Transitions
| From | Allowed To |
|------|------------|
| pending | confirmed, cancelled |
| confirmed | packed, cancelled |
| packed | shipped, cancelled |
| shipped | in_transit, delivered, returned |
| in_transit | delivered, returned |
| delivered | completed |
| cancelled | (final) |
| returned | refunded, reshipped |

### Payment Rules
- COD orders: Mark paid on delivery
- UPI orders: Auto-marked paid on transaction
- Credit orders: Can be partial payments

### Cancellation Rules
- Can cancel only before shipped
- If paid, trigger refund workflow
- Stock restored on cancellation

---

## Notifications Triggered

| Status Change | Buyer Notification |
|---------------|-------------------|
| Confirmed | "Your order is confirmed" |
| Packed | "Your order is packed" |
| Shipped | "Your order is shipped. Track: [link]" |
| Delivered | "Your order has been delivered" |
| Cancelled | "Your order has been cancelled. Reason: [X]" |

---

## Design Specifications

### Status Badge Colors
| Status | Color |
|--------|-------|
| Pending | Yellow |
| Confirmed | Blue |
| Packed | Purple |
| Shipped | Indigo |
| In Transit | Cyan |
| Delivered | Green |
| Completed | Dark Green |
| Cancelled | Red |
| Returned | Orange |

### Table Design
- Clickable rows
- Status badges prominent
- Amount right-aligned
- Date in relative format

### Details View
- Split layout: Info cards + Actions
- Collapsible timeline
- Sticky action buttons

---

## Checklist

### Development
- [ ] Create orders list page
- [ ] Implement status tabs with counts
- [ ] Add search functionality
- [ ] Add date and filter options
- [ ] Build orders table with pagination
- [ ] Create order details view
- [ ] Implement status update flow
- [ ] Add shipping info input
- [ ] Build payment update flow
- [ ] Create timeline visualization
- [ ] Implement invoice generation
- [ ] Add cancel order flow
- [ ] Create internal notes feature
- [ ] Add export functionality
- [ ] Connect all APIs

### Testing
- [ ] List loads correctly
- [ ] Tabs filter correctly
- [ ] Search works
- [ ] Filters work
- [ ] Status update works
- [ ] Shipping info saves
- [ ] Invoice generates
- [ ] Cancel order works
- [ ] Payment update works
- [ ] Timeline displays correctly
- [ ] Notifications sent

---

## Dependencies
- PDF generation library
- Date picker component
- Status workflow engine
- Notification service
- Export utility (CSV/Excel)
