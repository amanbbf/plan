# MongoDB Database Schema

## Overview

This document defines the MongoDB collections and schema for the B2B Grocery Wholesale Platform.

---

## Collections Summary

| Collection | Purpose |
|------------|---------|
| admins | Admin user accounts |
| buyers | Buyer accounts and business details |
| categories | Product categories |
| subcategories | Product sub-categories |
| products | Product catalog |
| carts | Shopping carts |
| orders | Order records |
| invoices | Invoice records |
| addresses | Buyer addresses |
| pincodes | Serviceable pin codes |
| couriers | Courier configurations |
| payments | Payment records |
| creditLedger | Credit transactions |
| notifications | Notification records |
| stockLogs | Inventory audit trail |
| settings | System settings |
| otps | OTP verification records |

---

## 1. Admins Collection

```javascript
{
  _id: ObjectId,
  name: String,                    // Full name
  email: String,                   // Unique, indexed
  password: String,                // Hashed
  phone: String,
  role: String,                    // "super_admin", "operations", "finance"
  status: String,                  // "active", "inactive"
  lastLogin: Date,
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `email`: unique
- `status`: regular

---

## 2. Buyers Collection

```javascript
{
  _id: ObjectId,
  phone: String,                   // Unique, indexed
  business: {
    name: String,                  // Required
    type: String,                  // "kirana", "hotel", "supermarket", etc.
    ownerName: String,
    email: String,
    gstNumber: String,             // Optional, validated
    panNumber: String              // Optional
  },
  address: {
    line1: String,
    line2: String,
    city: String,
    state: String,
    pincode: String,
    landmark: String
  },
  credit: {
    limit: Number,                 // Default: 0
    available: Number,             // Calculated
    outstanding: Number            // Current dues
  },
  status: String,                  // "pending", "active", "blocked", "rejected"
  approvedBy: ObjectId,            // Reference to admin
  approvedAt: Date,
  rejectionReason: String,
  staff: [{                        // Staff accounts (future)
    userId: ObjectId,
    name: String,
    phone: String,
    role: String                   // "staff", "viewer"
  }],
  deviceTokens: [String],          // Push notification tokens
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `phone`: unique
- `status`: regular
- `business.gstNumber`: sparse, unique
- `createdAt`: regular

---

## 3. Categories Collection

```javascript
{
  _id: ObjectId,
  name: String,                    // Unique
  slug: String,                    // URL-friendly, unique
  description: String,
  image: String,                   // URL
  icon: String,                    // Icon URL
  status: String,                  // "active", "inactive"
  sortOrder: Number,               // Display order
  productCount: Number,            // Cached count
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `name`: unique
- `slug`: unique
- `status, sortOrder`: compound

---

## 4. Subcategories Collection

```javascript
{
  _id: ObjectId,
  categoryId: ObjectId,            // Reference to category
  name: String,
  slug: String,
  description: String,
  image: String,
  status: String,                  // "active", "inactive"
  sortOrder: Number,
  productCount: Number,            // Cached count
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `categoryId`: regular
- `categoryId, name`: unique compound
- `slug`: unique

---

## 5. Products Collection

```javascript
{
  _id: ObjectId,
  name: String,                    // Required
  slug: String,                    // URL-friendly
  sku: String,                     // Unique
  description: String,
  brand: String,
  categoryId: ObjectId,            // Reference
  subcategoryId: ObjectId,         // Reference
  images: [{
    url: String,
    isPrimary: Boolean
  }],
  pricing: {
    mrp: Number,                   // Maximum retail price
    sellingPrice: Number,          // B2B price
    costPrice: Number,             // Internal
    gstRate: Number,               // 0, 5, 12, 18, 28
    hsnCode: String                // 4-8 digit HSN
  },
  inventory: {
    stock: Number,                 // Current stock
    lowStockAlert: Number,         // Alert threshold
    unit: String,                  // "piece", "kg", "box", "dozen", etc.
    weight: Number                 // In kg, for shipping
  },
  ordering: {
    moq: Number,                   // Minimum order quantity
    increment: Number,             // Order increment
    maxQty: Number                 // Max per order
  },
  bulkPricing: [{
    minQty: Number,
    maxQty: Number,                // null for unlimited
    price: Number,                 // Price per unit at this tier
    discountPercent: Number
  }],
  settings: {
    status: String,                // "active", "inactive"
    featured: Boolean,
    showStock: Boolean,
    allowBackorder: Boolean
  },
  stats: {
    orderCount: Number,            // Times ordered
    viewCount: Number              // Views (optional)
  },
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `sku`: unique
- `slug`: unique
- `categoryId`: regular
- `subcategoryId`: regular
- `settings.status`: regular
- `pricing.sellingPrice`: regular
- `name`: text (for search)
- `brand`: regular

---

## 6. Carts Collection

```javascript
{
  _id: ObjectId,
  buyerId: ObjectId,               // Reference to buyer
  items: [{
    productId: ObjectId,
    name: String,                  // Cached
    image: String,                 // Cached
    price: Number,                 // Price at time of adding
    appliedPrice: Number,          // After bulk discount
    quantity: Number,
    gstRate: Number,
    lineTotal: Number
  }],
  summary: {
    itemCount: Number,
    subtotal: Number,
    taxTotal: Number,
    total: Number
  },
  updatedAt: Date
}
```

**Indexes:**
- `buyerId`: unique

---

## 7. Orders Collection

```javascript
{
  _id: ObjectId,
  orderNumber: String,             // Sequential: ORD-2024-000001
  buyerId: ObjectId,               // Reference
  buyer: {                         // Snapshot
    businessName: String,
    ownerName: String,
    phone: String,
    gstNumber: String
  },
  items: [{
    productId: ObjectId,
    name: String,
    sku: String,
    hsnCode: String,
    image: String,
    price: Number,
    quantity: Number,
    gstRate: Number,
    gstAmount: Number,
    lineTotal: Number
  }],
  shippingAddress: {
    name: String,
    phone: String,
    line1: String,
    line2: String,
    city: String,
    state: String,
    pincode: String,
    landmark: String
  },
  pricing: {
    subtotal: Number,
    cgst: Number,
    sgst: Number,
    igst: Number,
    deliveryCharge: Number,
    discount: Number,
    total: Number
  },
  payment: {
    method: String,                // "upi", "cod", "credit"
    status: String,                // "pending", "paid", "partial", "failed"
    amountPaid: Number,
    transactionId: String,
    paidAt: Date
  },
  shipping: {
    courierId: ObjectId,
    courierName: String,
    awbNumber: String,
    shippedAt: Date,
    estimatedDelivery: Date,
    deliveredAt: Date,
    proofOfDelivery: String        // Image URL
  },
  status: String,                  // Order lifecycle status
  timeline: [{
    status: String,
    at: Date,
    by: ObjectId,                  // Admin who made change
    note: String
  }],
  invoice: {
    number: String,
    generatedAt: Date,
    url: String
  },
  notes: String,                   // Buyer notes
  adminNotes: [{
    text: String,
    by: ObjectId,
    at: Date
  }],
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `orderNumber`: unique
- `buyerId`: regular
- `status`: regular
- `createdAt`: regular
- `payment.status`: regular

---

## 8. Invoices Collection

```javascript
{
  _id: ObjectId,
  invoiceNumber: String,           // INV-2024-000001
  orderId: ObjectId,               // Reference
  orderNumber: String,
  seller: {
    name: String,
    address: Object,
    gstin: String,
    stateCode: String
  },
  buyer: {
    name: String,
    address: Object,
    gstin: String,
    stateCode: String
  },
  items: [{
    name: String,
    hsn: String,
    quantity: Number,
    rate: Number,
    taxableAmount: Number,
    gstRate: Number,
    cgst: Number,
    sgst: Number,
    igst: Number,
    total: Number
  }],
  summary: {
    subtotal: Number,
    cgst: Number,
    sgst: Number,
    igst: Number,
    deliveryCharge: Number,
    discount: Number,
    grandTotal: Number,
    amountInWords: String
  },
  invoiceDate: Date,
  dueDate: Date,
  pdfUrl: String,
  createdAt: Date
}
```

**Indexes:**
- `invoiceNumber`: unique
- `orderId`: regular
- `invoiceDate`: regular

---

## 9. Addresses Collection

```javascript
{
  _id: ObjectId,
  buyerId: ObjectId,               // Reference
  label: String,                   // "Shop", "Warehouse"
  name: String,                    // Contact name
  phone: String,
  line1: String,
  line2: String,
  city: String,
  state: String,
  pincode: String,
  landmark: String,
  isDefault: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `buyerId`: regular

---

## 10. Pincodes Collection

```javascript
{
  _id: ObjectId,
  pincode: String,                 // 6 digits, unique
  area: String,
  city: String,
  state: String,
  stateCode: String,
  zoneId: ObjectId,                // Optional zone reference
  zoneName: String,
  delivery: {
    daysMin: Number,
    daysMax: Number,
    charge: Number,
    freeAbove: Number              // Free delivery threshold
  },
  status: String,                  // "active", "inactive"
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `pincode`: unique
- `status`: regular
- `state`: regular

---

## 11. Couriers Collection

```javascript
{
  _id: ObjectId,
  name: String,                    // "India Post"
  code: String,                    // "INDPOST"
  trackingUrlTemplate: String,     // URL with {{awb}} placeholder
  contactPhone: String,
  contactEmail: String,
  status: String,                  // "active", "inactive"
  createdAt: Date,
  updatedAt: Date
}
```

**Indexes:**
- `code`: unique

---

## 12. Payments Collection

```javascript
{
  _id: ObjectId,
  orderId: ObjectId,
  buyerId: ObjectId,
  method: String,                  // "upi", "cod", "credit"
  amount: Number,
  status: String,                  // "pending", "success", "failed"
  transactionId: String,
  gatewayResponse: Object,         // Raw gateway response
  recordedBy: ObjectId,            // Admin (for manual)
  notes: String,
  paymentDate: Date,
  createdAt: Date
}
```

**Indexes:**
- `orderId`: regular
- `buyerId`: regular
- `status`: regular
- `paymentDate`: regular

---

## 13. Credit Ledger Collection

```javascript
{
  _id: ObjectId,
  buyerId: ObjectId,               // Reference
  type: String,                    // "debit", "credit"
  amount: Number,
  balanceBefore: Number,
  balanceAfter: Number,
  reference: {
    type: String,                  // "order", "payment", "adjustment"
    id: ObjectId,
    number: String                 // Order/payment number
  },
  description: String,
  recordedBy: ObjectId,            // Admin for manual entries
  createdAt: Date
}
```

**Indexes:**
- `buyerId`: regular
- `createdAt`: regular

---

## 14. Notifications Collection

```javascript
{
  _id: ObjectId,
  userId: ObjectId,                // Buyer or Admin ID
  userType: String,                // "buyer", "admin"
  type: String,                    // Notification type
  title: String,
  body: String,
  data: Object,                    // Action-specific data
  read: Boolean,
  readAt: Date,
  createdAt: Date
}
```

**Indexes:**
- `userId, userType`: compound
- `read`: regular
- `createdAt`: regular

---

## 15. Stock Logs Collection

```javascript
{
  _id: ObjectId,
  productId: ObjectId,
  type: String,                    // "add", "remove", "set"
  quantity: Number,
  stockBefore: Number,
  stockAfter: Number,
  reason: String,                  // "purchase", "sale", "damaged", etc.
  reference: {
    type: String,                  // "order", "manual", "return"
    id: ObjectId
  },
  notes: String,
  adjustedBy: ObjectId,            // Admin
  createdAt: Date
}
```

**Indexes:**
- `productId`: regular
- `createdAt`: regular

---

## 16. Settings Collection

```javascript
{
  _id: "settings",                 // Single document
  company: {
    name: String,
    legalName: String,
    gstin: String,
    pan: String,
    address: Object,
    phone: String,
    email: String,
    website: String,
    logo: String,
    bank: {
      name: String,
      accountNumber: String,
      ifsc: String,
      branch: String,
      upiId: String
    }
  },
  tax: {
    defaultRate: Number,
    stateCode: String,
    invoicePrefix: String,
    invoiceStartNumber: Number,
    fyStart: String
  },
  ordering: {
    minOrderValue: Number,
    maxOrderValue: Number,
    defaultMoq: Number,
    allowBackorder: Boolean
  },
  credit: {
    defaultLimit: Number,
    maxLimit: Number,
    period: Number,
    blockOnOverdue: Boolean
  },
  delivery: {
    defaultCharge: Number,
    freeAbove: Number,
    maxDays: Number
  },
  notifications: {
    email: Object,
    push: Object,
    sms: Object
  },
  counters: {
    orderNumber: Number,
    invoiceNumber: Number
  },
  updatedAt: Date
}
```

---

## 17. OTPs Collection

```javascript
{
  _id: ObjectId,
  phone: String,
  otp: String,                     // Hashed
  purpose: String,                 // "login", "register"
  expiresAt: Date,
  attempts: Number,
  verified: Boolean,
  createdAt: Date
}
```

**Indexes:**
- `phone, purpose`: compound
- `expiresAt`: TTL index (auto-delete expired)

---

## Relationships Diagram

```
admins ─────────────────────────────────┐
                                        │
buyers ─────┬───────────────────────────┼── approvedBy
            │                           │
            ├── carts (1:1)             │
            ├── addresses (1:many)      │
            ├── orders (1:many) ────────┼── processedBy
            ├── creditLedger (1:many)   │
            ├── notifications (1:many)  │
            └── payments (1:many)       │
                                        │
categories ─┬── subcategories (1:many)  │
            │                           │
subcategories ─┬── products (1:many)    │
               │                        │
products ──────┼── orders.items         │
               ├── carts.items          │
               └── stockLogs (1:many)   │
                                        │
orders ────────┬── invoices (1:1)       │
               ├── payments (1:many)    │
               └── shipping ─── couriers│
                                        │
pincodes ──────── serviceability check  │
                                        │
settings ──────── global config         │
```

---

## Data Migration Notes

### Initial Data Required
1. Admin user (super_admin)
2. Company settings
3. GST rates
4. At least one category/subcategory
5. Serviceable pin codes
6. At least one courier

### Seed Data Script
Create script to:
- Insert default admin
- Set initial settings
- Add sample categories
- Add sample pin codes

---

## Backup Strategy

### Critical Collections
- orders
- invoices
- payments
- creditLedger
- buyers

### Regular Backups
- Daily automated backups
- Point-in-time recovery
- Off-site backup storage

---

## Performance Considerations

### Indexing Strategy
- Index frequently queried fields
- Compound indexes for common queries
- Text indexes for search

### Aggregation
- Use aggregation for reports
- Cache dashboard stats

### Data Archiving
- Archive old orders (>1 year)
- Archive notifications (>3 months)
