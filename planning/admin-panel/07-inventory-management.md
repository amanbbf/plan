# Admin Panel - Inventory Management Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Inventory Management |
| URL | `/admin/inventory` |
| Access | Super Admin, Operations |
| Priority | P1 - Important |
| Responsive | Desktop only |

---

## Purpose
Track stock levels, manage stock adjustments, view inventory history, and handle low stock alerts.

---

## UI Components

### 1. Page Header
- Title: "Inventory Management"
- Search bar (product name, SKU)
- "Stock Adjustment" button
- "Export Report" button

### 2. Quick Stats Cards
| Stat | Description |
|------|-------------|
| Total Products | Count of all products |
| In Stock | Products with stock > 0 |
| Low Stock | Products below threshold |
| Out of Stock | Products with stock = 0 |

### 3. Filter Bar
| Filter | Options |
|--------|---------|
| Category | All categories dropdown |
| Stock Status | All, In Stock, Low Stock, Out of Stock |
| Sort By | Name, Stock (Low-High), Stock (High-Low) |

### 4. Inventory Table
| Column | Description | Sortable |
|--------|-------------|----------|
| Product | Image + Name + SKU | Yes |
| Category | Category name | Yes |
| Current Stock | Quantity | Yes |
| Low Stock Alert | Threshold | No |
| Unit | Piece/Kg/Box etc | No |
| Last Updated | Date of last change | Yes |
| Status | In Stock / Low / Out | No |
| Actions | Update, History | No |

---

## Stock Update Modal

### Update Type Options
- **Add Stock** - Increase quantity
- **Remove Stock** - Decrease quantity
- **Set Stock** - Override to specific value

### Form Fields
| Field | Type | Required |
|-------|------|----------|
| Product | Display only | - |
| Current Stock | Display only | - |
| Adjustment Type | Dropdown | Yes |
| Quantity | Number input | Yes |
| Reason | Dropdown | Yes |
| Notes | Text area | No |
| Reference | Text (invoice no.) | No |

### Reason Options
| Reason | Type |
|--------|------|
| Purchase / Restock | Add |
| Sale Return | Add |
| Damaged Goods | Remove |
| Sale (Manual) | Remove |
| Expired Stock | Remove |
| Stock Correction | Add/Remove |
| Opening Stock | Set |

---

## Stock History View

### Per Product History
| Column | Description |
|--------|-------------|
| Date/Time | When adjustment made |
| Type | Add/Remove/Set |
| Quantity | Change amount |
| Before | Stock before |
| After | Stock after |
| Reason | Reason selected |
| By | Admin who made change |
| Notes | Additional notes |

### Filters
- Date range
- Adjustment type
- Admin user

---

## Bulk Stock Update

### CSV Upload Option
1. Download template
2. Fill product SKUs and quantities
3. Upload CSV
4. Review changes
5. Confirm bulk update

### Template Format
```csv
sku,quantity,type,reason
BIS-PG-800,100,add,purchase
SNK-LAYS-50,50,add,purchase
```

---

## API Calls

### GET `/api/admin/inventory`
List all products with stock info.
**Query:** page, limit, category, status, search, sortBy

### GET `/api/admin/inventory/:productId/history`
Stock history for specific product.

### POST `/api/admin/inventory/:productId/adjust`
Adjust stock for a product.
```json
{
  "type": "add",
  "quantity": 100,
  "reason": "purchase",
  "notes": "New stock from supplier",
  "reference": "PO-2024-001"
}
```

### POST `/api/admin/inventory/bulk-adjust`
Bulk stock adjustment via CSV data.

### GET `/api/admin/inventory/low-stock`
Get all low stock products.

### GET `/api/admin/inventory/export`
Export inventory report.

---

## Stock Adjustment Data Structure

```json
{
  "_id": "adj_123",
  "product": "prod_123",
  "type": "add",
  "quantity": 100,
  "beforeStock": 50,
  "afterStock": 150,
  "reason": "purchase",
  "notes": "Supplier delivery",
  "reference": "PO-2024-001",
  "adjustedBy": "admin_123",
  "adjustedAt": "2024-01-15T10:00:00Z"
}
```

---

## Low Stock Alerts

### Configuration (per product)
- Low stock threshold (default: 10)
- Enable/disable alerts

### Alert Display
- Row highlighted in orange/red
- Dashboard widget shows count
- Optional: Email notification

### Out of Stock Handling
- Row highlighted in red
- Product hidden from buyer catalog (optional)
- Prominent alert on dashboard

---

## Business Rules

### Stock Validation
- Stock cannot go negative
- Warn if adjustment makes stock zero
- Confirm before setting stock to zero

### Audit Trail
- All changes logged automatically
- Cannot edit/delete history
- Admin user recorded for each change

### Auto Stock Updates
- Order placed: Stock reserved
- Order confirmed: Stock deducted
- Order cancelled: Stock restored
- Return processed: Stock added back

---

## Design Specifications

### Status Indicators
| Status | Color | Icon |
|--------|-------|------|
| In Stock | Green | ✓ |
| Low Stock | Orange | ⚠ |
| Out of Stock | Red | ✗ |

### Table Design
- Conditional row highlighting based on stock status
- Quick inline stock update option
- Hover to show last update details

---

## Checklist

### Development
- [ ] Create inventory list page
- [ ] Implement stock status filters
- [ ] Build stock update modal
- [ ] Add adjustment reasons
- [ ] Create stock history view
- [ ] Implement bulk update
- [ ] Add low stock alerts
- [ ] Create export functionality
- [ ] Connect auto-deduction on orders
- [ ] Add audit trail logging

### Testing
- [ ] List displays correctly
- [ ] Filters work
- [ ] Stock add works
- [ ] Stock remove works
- [ ] Cannot go negative
- [ ] History records correctly
- [ ] Bulk update works
- [ ] Export works
- [ ] Order auto-deduction works

---

## Dependencies
- Product management system
- Order management system
- CSV parser
- Export utility
- Notification service (for alerts)
