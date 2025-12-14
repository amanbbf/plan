# Admin Panel - Shipping Management Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Shipping Management |
| URL | `/admin/shipping` |
| Access | Super Admin, Operations |
| Priority | P0 - Critical |
| Responsive | Desktop only |

---

## Purpose
Manage shipments, assign couriers and tracking numbers, update delivery status, and track all dispatched orders.

---

## UI Components

### 1. Page Header
- Title: "Shipping Management"
- Search (order ID, AWB number)
- Date range filter
- "Ready to Ship" count badge

### 2. Quick Stats
| Stat | Description |
|------|-------------|
| Ready to Ship | Packed, awaiting dispatch |
| In Transit | Currently shipping |
| Delivered Today | Today's deliveries |
| Pending Delivery | Overdue shipments |

### 3. Tabs
| Tab | Filter |
|-----|--------|
| Ready to Ship | Status = packed |
| In Transit | Status = shipped, in_transit |
| Delivered | Status = delivered |
| Returns | Status = returned |
| All Shipments | No filter |

### 4. Shipments Table
| Column | Description | Sortable |
|--------|-------------|----------|
| Order ID | Order number | Yes |
| Buyer | Business name + City | Yes |
| Items | Item count | No |
| Packed Date | When packed | Yes |
| Courier | Assigned courier | Yes |
| AWB Number | Tracking number | No |
| Ship Date | When dispatched | Yes |
| Est. Delivery | Expected date | Yes |
| Status | Current status | Yes |
| Actions | Ship, Update, Track | No |

---

## Ship Order Modal

When clicking "Ship" on a packed order:

### Form Fields
| Field | Type | Required |
|-------|------|----------|
| Courier Service | Dropdown | Yes |
| AWB Number | Text | Yes |
| Shipping Date | Date picker | Yes |
| Estimated Delivery | Date picker | Yes |
| Package Weight | Number (kg) | No |
| Package Dimensions | L x W x H | No |
| Shipping Notes | Text area | No |

### Courier Options (Configurable)
- India Post - Speed Post
- India Post - Regular
- DTDC
- Delhivery
- Blue Dart
- Local Courier
- Self Delivery

---

## Update Shipment Status

### Status Options
| Status | Description |
|--------|-------------|
| shipped | Handed to courier |
| in_transit | On the way |
| out_for_delivery | With delivery agent |
| delivered | Successfully delivered |
| delivery_failed | Attempt failed |
| returned | Returned to sender |

### Update Form
| Field | Type | Required |
|-------|------|----------|
| New Status | Dropdown | Yes |
| Update Date | Date picker | Yes |
| Notes | Text area | No |
| Proof of Delivery | File upload | No (for delivered) |

---

## Shipment Details View

### Order Information
- Order ID, date, buyer details
- Delivery address with pin code
- Items summary

### Shipping Information
| Field | Value |
|-------|-------|
| Courier | Service name |
| AWB Number | Tracking number |
| Shipped On | Date |
| Expected Delivery | Date |
| Current Status | Status badge |
| Last Update | Date/time |

### Tracking Timeline
Visual timeline:
```
[✓] Order Packed - Jan 15, 10:00 AM
[✓] Handed to Courier - Jan 15, 2:00 PM
[✓] In Transit - Jan 16, 9:00 AM
[○] Out for Delivery
[○] Delivered
```

### External Tracking
- "Track on Courier Website" button
- Opens courier tracking page in new tab

---

## Courier Configuration

### Manage Couriers Page
`/admin/settings/couriers`

| Column | Description |
|--------|-------------|
| Courier Name | Display name |
| Tracking URL Template | URL with {{awb}} placeholder |
| Contact | Phone/email |
| Status | Active/Inactive |

### Add Courier
| Field | Type | Example |
|-------|------|---------|
| Name | Text | "India Post" |
| Short Code | Text | "INDPOST" |
| Tracking URL | Text | `https://www.indiapost.gov.in/vas/Pages/Tracking.aspx?search={{awb}}` |
| Contact Phone | Text | "1800-xxx-xxx" |
| Status | Toggle | Active |

---

## API Calls

### GET `/api/admin/shipments`
List shipments with filters.

### GET `/api/admin/shipments/:orderId`
Get shipment details for an order.

### POST `/api/admin/shipments/:orderId/ship`
Create shipment / mark as shipped.
```json
{
  "courier": "india_post",
  "awbNumber": "EE123456789IN",
  "shippedDate": "2024-01-15",
  "estimatedDelivery": "2024-01-20",
  "weight": 2.5,
  "notes": "Fragile items inside"
}
```

### PUT `/api/admin/shipments/:orderId/status`
Update shipment status.
```json
{
  "status": "delivered",
  "updateDate": "2024-01-18",
  "notes": "Received by shop owner",
  "proofOfDelivery": "image_url"
}
```

### Couriers

### GET `/api/admin/couriers`
List configured couriers.

### POST `/api/admin/couriers`
Add new courier.

### PUT `/api/admin/couriers/:id`
Update courier.

---

## Shipment Data Structure

```json
{
  "_id": "ship_123",
  "order": "order_123",
  "courier": {
    "id": "courier_indpost",
    "name": "India Post",
    "trackingUrl": "https://..."
  },
  "awbNumber": "EE123456789IN",
  "shippedAt": "2024-01-15T14:00:00Z",
  "estimatedDelivery": "2024-01-20",
  "deliveredAt": null,
  "weight": 2.5,
  "dimensions": {
    "length": 30,
    "width": 20,
    "height": 15
  },
  "status": "in_transit",
  "timeline": [
    { "status": "shipped", "at": "...", "by": "admin_123" },
    { "status": "in_transit", "at": "...", "by": "admin_123" }
  ],
  "proofOfDelivery": null,
  "notes": "Fragile items"
}
```

---

## Bulk Ship Feature

### Process
1. Select multiple packed orders
2. Click "Bulk Ship"
3. Select common courier
4. Enter AWB numbers for each (or upload CSV)
5. Set shipping date
6. Confirm

### CSV Format for AWB
```csv
orderId,awbNumber
ORD-2024-001,EE123456789IN
ORD-2024-002,EE123456790IN
```

---

## Delivery Notifications

| Status Change | Buyer Notification |
|---------------|-------------------|
| Shipped | "Your order has been shipped. Track: [AWB]" |
| Out for Delivery | "Your order is out for delivery" |
| Delivered | "Your order has been delivered" |
| Delivery Failed | "Delivery attempted. Will retry." |

---

## Print Labels (Optional)

### Shipping Label Content
- From address (seller)
- To address (buyer)
- Order ID
- AWB number
- Barcode
- Item summary

### Format
- A5 or 4x6 inch
- Print-ready PDF

---

## Checklist

### Development
- [ ] Create shipments list page
- [ ] Implement status tabs
- [ ] Build ship order modal
- [ ] Add courier selection
- [ ] Create AWB entry
- [ ] Build status update flow
- [ ] Create tracking timeline
- [ ] Add external tracking link
- [ ] Implement courier configuration
- [ ] Add bulk ship feature
- [ ] Create shipping notifications
- [ ] Build print label feature (optional)

### Testing
- [ ] Packed orders show in Ready to Ship
- [ ] Ship order flow works
- [ ] AWB saves correctly
- [ ] Status updates work
- [ ] Timeline displays correctly
- [ ] External tracking links work
- [ ] Notifications sent
- [ ] Bulk ship works

---

## Dependencies
- Order management system
- Courier configuration
- Notification service
- PDF generation (for labels)
- Timeline component
