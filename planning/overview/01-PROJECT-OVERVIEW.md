# B2B Grocery Wholesale Platform - Project Overview

## One-Line Summary
"A small-district B2B grocery wholesale platform using pin-code based delivery and third-party couriers like India Post, with admin-controlled inventory, GST billing, MOQ ordering, and a simple buyer ordering experience."

---

## 📌 Business Context

### Business Model
- **Type:** Single company / inventory-led model (NOT marketplace)
- **Geographic Scope:** One district + nearby towns
- **Scale:** Small-scale, cost-efficient platform
- **Industry:** B2B Grocery & FMCG wholesale

### Target Customers
- Kirana stores
- Small retailers
- Hotels & restaurants
- Small businesses buying in bulk

### Delivery Model
- Pin-code based serviceability
- Third-party couriers (India Post, private couriers)
- No in-house delivery fleet
- Admin manually assigns tracking

---

## 🎯 MVP Scope

### What's IN MVP
| Feature | Priority |
|---------|----------|
| Admin Panel (Full) | P0 - Critical |
| Buyer Ordering Flow | P0 - Critical |
| GST Invoicing | P0 - Critical |
| MOQ & Bulk Ordering | P0 - Critical |
| Pin-code Serviceability | P0 - Critical |
| Order Status Tracking | P0 - Critical |
| Simple Credit Ledger | P1 - Important |
| UPI / COD Payments | P1 - Important |

### What's NOT in MVP
- ❌ Seller marketplace
- ❌ Delivery boy app
- ❌ Google Maps / GPS tracking
- ❌ Live delivery tracking
- ❌ RFQ (Request for Quote)
- ❌ Multi-warehouse routing
- ❌ Dynamic pricing engine
- ❌ Hyperlocal slot delivery
- ❌ Complex payment automation

---

## 👥 User Types

### 1. Buyers (Business Customers)
| Role | Permissions |
|------|-------------|
| Store Owner (Admin) | Full access - order, view, manage staff, view invoices |
| Staff | Limited - place orders, view orders only |

### 2. Admin Users
| Role | Permissions |
|------|-------------|
| Super Admin | Full system access |
| Operations | Products, orders, inventory, shipments |
| Finance | Payments, invoices, credit management |

---

## 🖥️ Platform Strategy

### MVP Platforms
| User Type | Platform | Priority |
|-----------|----------|----------|
| Admin | Web Panel Only | P0 |
| Buyer | Android App OR Responsive Web | P0 |
| Seller | Not applicable | - |
| Delivery | Not applicable | - |

### Tech Stack (Planned)
| Layer | Technology |
|-------|------------|
| Frontend (Buyer) | Expo (React Native) |
| Frontend (Admin) | React.js Web |
| Backend | Node.js / Express.js |
| Database | MongoDB |
| Authentication | JWT |
| File Storage | Cloud storage (S3/Cloudinary) |

---

## 💰 Payment Model

### Supported Payment Methods
1. **UPI** - Online payment via UPI apps
2. **COD** - Cash on Delivery
3. **Manual Credit** - Admin-managed credit line

### Payment Features
- Simple payment status tracking
- No complex payment gateway automation in MVP
- Manual reconciliation by admin

---

## 📦 Pricing Model

### Pricing Structure
- **Fixed Pricing** - Same price for all buyers
- **MOQ Based** - Minimum Order Quantity per product
- **Simple Bulk Discounts** - Buy X get Y% off (admin configured)

### What's NOT Included
- No customer-specific pricing
- No dynamic pricing engine
- No real-time price updates

---

## 🚚 Logistics Model

### Serviceability
- Pin-code based delivery zones
- Admin defines serviceable pin codes
- Estimated delivery days per zone

### Shipping Process
1. Order placed by buyer
2. Admin processes order
3. Admin assigns courier + AWB number
4. Buyer sees tracking status
5. Delivery confirmed

### Tracking
- Manual tracking number entry
- External courier tracking links
- Simple status updates (Packed → Shipped → Delivered)

---

## 📋 Key B2B Features

### GST Compliance
- GST number validation for buyers
- HSN codes on products
- GST-compliant invoices
- CGST/SGST/IGST breakup

### Order Management
- MOQ enforcement
- Bulk quantity ordering
- Order status lifecycle
- Order history

### Credit Management
- Simple credit ledger
- Manual credit limit assignment
- Payment due tracking
- Outstanding balance view

---

## 🔄 Core Workflow

```
[Buyer Registration] 
    ↓
[Admin Approval]
    ↓
[Buyer Browses Products]
    ↓
[Adds to Cart (MOQ enforced)]
    ↓
[Checkout (Address + Payment)]
    ↓
[Order Created]
    ↓
[Admin Processes Order]
    ↓
[Invoice Generated (GST)]
    ↓
[Admin Assigns Courier + AWB]
    ↓
[Buyer Tracks Shipment]
    ↓
[Delivery Confirmed]
    ↓
[Payment Reconciliation]
```

---

## 📊 MVP Page Count Summary

| Section | Page Count |
|---------|------------|
| Admin Panel | ~20 pages |
| Buyer App | ~15 pages |
| **Total** | **~35 pages** |

---

## 🚀 Development Priority

### Phase 1: Foundation (Week 1-2)
- Database setup
- Authentication system
- Admin basic structure

### Phase 2: Admin Panel (Week 2-4)
- Dashboard
- Product management
- Order management
- User management

### Phase 3: Buyer App (Week 4-6)
- Registration/Login
- Product browsing
- Cart & Checkout
- Order tracking

### Phase 4: Integration (Week 6-7)
- Invoice generation
- Shipping integration
- Payment integration

### Phase 5: Testing & Launch (Week 7-8)
- Testing
- Bug fixes
- Soft launch

---

## 📁 Document Structure

```
planning/
├── overview/
│   ├── 01-PROJECT-OVERVIEW.md (this file)
│   └── 02-MVP-FEATURES.md
├── admin-panel/
│   ├── 01-admin-login.md
│   ├── 02-admin-dashboard.md
│   ├── ... (all admin pages)
├── buyer-app/
│   ├── 01-buyer-login.md
│   ├── 02-buyer-registration.md
│   ├── ... (all buyer pages)
├── database/
│   └── 01-mongodb-schema.md
├── api/
│   └── 01-api-endpoints.md
└── checklists/
    ├── 01-development-checklist.md
    └── 02-testing-checklist.md
```

---

## ✅ Success Criteria

1. Admin can manage full catalog
2. Admin can process orders end-to-end
3. Buyers can register and get approved
4. Buyers can browse, order, and track
5. GST invoices generated correctly
6. Pin-code serviceability works
7. Simple credit tracking works
8. Basic analytics visible to admin
