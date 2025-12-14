# Buyer App - Order Details Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Order Details |
| Route | `/orders/:id` or `OrderDetailScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
View complete order information including items, status, tracking, delivery details, and invoice.

---

## UI Components

### 1. Header
- Back button
- Title: "Order #ORD-2024-0001"
- More options (menu)

### 2. Status Section
```
┌─────────────────────────────────────┐
│ ● Shipped                           │
│ Your order is on the way!           │
│                                     │
│ Estimated delivery: 20 Jan 2024     │
└─────────────────────────────────────┘
```

### 3. Order Timeline
```
✓ Order Placed      15 Jan, 10:00 AM
✓ Confirmed         15 Jan, 11:30 AM
✓ Packed            16 Jan, 2:00 PM
✓ Shipped           16 Jan, 4:30 PM
○ Out for Delivery
○ Delivered
```

### 4. Tracking Info (if shipped)
```
┌─────────────────────────────────────┐
│ Courier: India Post                 │
│ Tracking #: EE123456789IN           │
│                                     │
│ [Track on Courier Website]          │
└─────────────────────────────────────┘
```

### 5. Order Items
```
┌─────────────────────────────────────┐
│ [Img] Parle-G 800g                  │
│       24 × ₹40 = ₹960               │
├─────────────────────────────────────┤
│ [Img] Britannia Marie               │
│       12 × ₹35 = ₹420               │
└─────────────────────────────────────┘
```

### 6. Price Summary
```
Subtotal:        ₹5,000
CGST (2.5%):       ₹125
SGST (2.5%):       ₹125
Delivery:          Free
─────────────────────────
Total:           ₹5,250
```

### 7. Delivery Address
```
┌─────────────────────────────────────┐
│ Krishna Stores                      │
│ 123 Main Road, Near Bus Stand       │
│ Bangalore, Karnataka 560001         │
│ Phone: 9876543210                   │
└─────────────────────────────────────┘
```

### 8. Payment Info
```
┌─────────────────────────────────────┐
│ Method: UPI                         │
│ Status: Paid                        │
│ Transaction ID: TXN123456           │
└─────────────────────────────────────┘
```

### 9. Actions (based on status)
```
┌────────────────────────────────────┐
│ [Download Invoice]  [Reorder]      │
└────────────────────────────────────┘
```

---

## Order Data

```json
{
  "id": "order_123",
  "orderNumber": "ORD-2024-0001",
  "createdAt": "2024-01-15T10:00:00Z",
  "status": "shipped",
  "items": [
    {
      "product": {
        "id": "prod_1",
        "name": "Parle-G 800g",
        "image": "url"
      },
      "quantity": 24,
      "price": 40,
      "total": 960
    }
  ],
  "pricing": {
    "subtotal": 5000,
    "cgst": 125,
    "sgst": 125,
    "igst": 0,
    "deliveryCharge": 0,
    "total": 5250
  },
  "address": {
    "name": "Krishna Stores",
    "line1": "123 Main Road",
    "line2": "Near Bus Stand",
    "city": "Bangalore",
    "state": "Karnataka",
    "pincode": "560001",
    "phone": "9876543210"
  },
  "payment": {
    "method": "upi",
    "status": "paid",
    "transactionId": "TXN123456"
  },
  "shipping": {
    "courier": "India Post",
    "awbNumber": "EE123456789IN",
    "trackingUrl": "https://...",
    "shippedAt": "2024-01-16T16:30:00Z",
    "estimatedDelivery": "2024-01-20"
  },
  "timeline": [
    { "status": "pending", "at": "...", "label": "Order Placed" },
    { "status": "confirmed", "at": "...", "label": "Confirmed" },
    { "status": "packed", "at": "...", "label": "Packed" },
    { "status": "shipped", "at": "...", "label": "Shipped" }
  ],
  "invoice": {
    "number": "INV-2024-0001",
    "url": "https://..."
  },
  "notes": "Please deliver before 5 PM"
}
```

---

## API Calls

### GET `/api/orders/:id`
Get complete order details.

### GET `/api/orders/:id/invoice`
Download invoice PDF.

### POST `/api/orders/:id/cancel`
Cancel order (if eligible).

---

## Actions by Status

| Status | Available Actions |
|--------|------------------|
| pending | Cancel Order |
| confirmed | Cancel Order, Track |
| packed | Track |
| shipped | Track, Track External |
| delivered | Invoice, Reorder |
| cancelled | Reorder |

---

## Track Order

### In-App Tracking
- Timeline view
- Status badges
- Dates and times

### External Tracking
- "Track on Courier Website" button
- Opens courier tracking URL
- Passing AWB number

---

## Cancel Order Flow

Only available before shipping.

1. User taps "Cancel Order"
2. Show confirmation with reason selection
3. Reasons: Changed mind, Found elsewhere, Order by mistake, Other
4. Confirm cancellation
5. Update order status
6. Show refund info (if paid)

---

## Invoice Download

1. User taps "Download Invoice"
2. Fetch invoice PDF
3. Open in browser or download
4. Option to share

---

## Reorder Flow

1. User taps "Reorder"
2. Add all order items to cart
3. Navigate to cart
4. Show confirmation

---

## Design Specifications

### Layout
- Scrollable content
- Section cards
- Fixed bottom actions (optional)

### Timeline
- Vertical line connecting steps
- Completed: Green checkmark
- Current: Highlighted dot
- Pending: Gray circle

### Status Card
- Large status badge
- Status message
- Estimated delivery

### Item List
- Thumbnail images
- Quantity × price
- Line totals

### Sections
- Clear headings
- Card containers
- Consistent padding

---

## Checklist

### Development
- [ ] Create order details screen
- [ ] Fetch order by ID
- [ ] Display status section
- [ ] Build order timeline
- [ ] Show tracking info
- [ ] Display order items
- [ ] Show price summary
- [ ] Show delivery address
- [ ] Show payment info
- [ ] Implement invoice download
- [ ] Implement reorder
- [ ] Implement cancel order
- [ ] External tracking link
- [ ] Loading and error states

### Testing
- [ ] Order details load
- [ ] Timeline displays correctly
- [ ] Status correct
- [ ] Tracking info shows
- [ ] Invoice downloads
- [ ] Reorder works
- [ ] Cancel works (when eligible)
- [ ] All sections render

---

## Dependencies
- Order API
- PDF viewer/downloader
- External link handler
- Cart state (reorder)
- Timeline component
