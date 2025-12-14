# Admin Panel - Product Management Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Product Management |
| URL | `/admin/products` |
| Access | Super Admin, Operations |
| Priority | P0 - Critical |
| Responsive | Desktop only |

---

## Purpose
Complete product catalog management - add, edit, delete products with all B2B-specific attributes like HSN codes, GST rates, MOQ, and bulk pricing.

---

## UI Components

### 1. Page Header
- Title: "Product Management"
- Search bar (search by name, SKU, HSN)
- "Add Product" button (primary)
- "Bulk Upload" button (secondary)

### 2. Filter Bar
| Filter | Options |
|--------|---------|
| Category | Dropdown (all categories) |
| Sub-category | Dropdown (based on category) |
| Status | All, Active, Inactive, Out of Stock |
| Stock Level | All, Low Stock, In Stock |

### 3. Products Table
| Column | Description | Sortable |
|--------|-------------|----------|
| Image | Product thumbnail | No |
| Product Name | Name + SKU | Yes |
| Category | Category > Sub-cat | Yes |
| HSN | HSN code | No |
| MRP | Maximum retail price | Yes |
| Selling Price | B2B price | Yes |
| GST | Tax rate % | No |
| Stock | Current quantity | Yes |
| MOQ | Minimum order qty | No |
| Status | Active/Inactive | Yes |
| Actions | Edit, Duplicate, Delete | No |

### 4. Bulk Actions (when rows selected)
- Set Active
- Set Inactive
- Update Stock
- Delete Selected

### 5. Pagination
- 25/50/100 rows per page
- Page navigation

---

## Add/Edit Product Form

### Multi-tab Form Layout

**Tab 1: Basic Information**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Product Name | Text | Max 100 chars | Yes |
| SKU | Text | Unique, alphanumeric | Yes |
| Description | Rich text | Max 1000 chars | No |
| Category | Dropdown | - | Yes |
| Sub-category | Dropdown | - | Yes |
| Brand | Text | Max 50 chars | No |

**Tab 2: Pricing & Tax**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| MRP | Number | > 0 | Yes |
| Selling Price | Number | > 0, <= MRP | Yes |
| Cost Price | Number | > 0 | No (internal) |
| GST Rate | Dropdown | 0%, 5%, 12%, 18%, 28% | Yes |
| HSN Code | Text | 4-8 digits | Yes |

**Tab 3: Inventory**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Stock Quantity | Number | >= 0 | Yes |
| Low Stock Alert | Number | >= 0 | No |
| Unit | Dropdown | Piece, Kg, Box, Dozen, etc. | Yes |
| Weight | Number | For shipping calc | No |

**Tab 4: Ordering Rules**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| MOQ | Number | >= 1 | Yes (default: 1) |
| Order Increment | Number | >= 1 | No (default: 1) |
| Max Qty Per Order | Number | >= MOQ | No |

**Tab 5: Bulk Pricing (Optional)**
| Min Qty | Max Qty | Discount % |
|---------|---------|------------|
| 10 | 24 | 5% |
| 25 | 49 | 8% |
| 50+ | - | 10% |

**Tab 6: Images**
- Primary image (required)
- Additional images (up to 5)
- Drag to reorder
- Image requirements: JPG/PNG, max 2MB, min 500x500px

**Tab 7: Settings**
| Field | Type | Default |
|-------|------|---------|
| Status | Toggle | Active |
| Featured | Toggle | No |
| Show Stock | Toggle | Yes |
| Allow Backorder | Toggle | No |

---

## Product Data Structure

```json
{
  "_id": "prod_123",
  "name": "Parle-G Biscuit 800g",
  "sku": "BIS-PARLE-800",
  "description": "Family pack biscuits",
  "category": "cat_snacks",
  "subcategory": "subcat_biscuits",
  "brand": "Parle",
  "images": [
    { "url": "...", "isPrimary": true },
    { "url": "...", "isPrimary": false }
  ],
  "pricing": {
    "mrp": 50,
    "sellingPrice": 42,
    "costPrice": 38,
    "gstRate": 5,
    "hsnCode": "1905"
  },
  "inventory": {
    "stock": 500,
    "lowStockAlert": 50,
    "unit": "pack",
    "weight": 0.8
  },
  "ordering": {
    "moq": 12,
    "increment": 6,
    "maxQty": 100
  },
  "bulkPricing": [
    { "minQty": 24, "maxQty": 47, "discountPercent": 5 },
    { "minQty": 48, "maxQty": null, "discountPercent": 8 }
  ],
  "settings": {
    "status": "active",
    "featured": false,
    "showStock": true,
    "allowBackorder": false
  },
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": "2024-01-15T00:00:00Z"
}
```

---

## API Calls

### GET `/api/admin/products`
List products with filters and pagination.

**Query Params:**
- `page`, `limit`
- `category`, `subcategory`
- `status` - active/inactive/outofstock
- `search` - text search
- `sortBy`, `sortOrder`

### POST `/api/admin/products`
Create new product.

### GET `/api/admin/products/:id`
Get single product details.

### PUT `/api/admin/products/:id`
Update product.

### DELETE `/api/admin/products/:id`
Delete product (soft delete).

### POST `/api/admin/products/:id/duplicate`
Duplicate product with new SKU.

### PUT `/api/admin/products/bulk-update`
Bulk update multiple products.

### POST `/api/admin/products/bulk-upload`
Upload products via CSV.

---

## Validation Rules

### Product Name
- Required
- 3-100 characters
- No special characters except -,&

### SKU
- Required
- Unique across all products
- Alphanumeric with hyphens
- Auto-generate option

### Pricing
- Selling price <= MRP
- Cost price < Selling price (warning, not blocking)
- All prices > 0

### HSN Code
- Required
- 4-8 numeric digits
- Validate against HSN database (optional)

### Stock
- Non-negative integer
- Update creates stock log entry

### MOQ
- Must be >= 1
- Increment must divide evenly into MOQ

---

## Stock Management

### Stock Update Modal
When clicking stock quantity:
- Current stock display
- Adjustment type: Add / Remove / Set
- Quantity input
- Reason dropdown (Purchase, Sale, Damaged, Return, Correction)
- Notes field
- Submit creates audit log entry

### Low Stock Alerts
- Products below threshold shown with warning icon
- Dashboard widget shows low stock count
- Optional: Email notification to admin

---

## Bulk Upload

### CSV Format
```csv
name,sku,category,subcategory,mrp,sellingPrice,gstRate,hsnCode,stock,moq,unit
"Parle-G 800g","BIS-PG-800","Snacks","Biscuits",50,42,5,1905,500,12,pack
```

### Upload Process
1. Download template CSV
2. Fill product data
3. Upload CSV file
4. System validates data
5. Shows validation results
6. Confirm to import

### Validation on Upload
- Check required fields
- Validate category/subcategory exist
- Check SKU uniqueness
- Validate number formats
- Show row-by-row errors

---

## Design Specifications

### Table
- Compact view for more products
- Product image: 40x40px thumbnail
- Price formatting: ₹XX.XX
- Stock colors: Red if low, Green if good
- Action icons on hover

### Form
- Multi-tab layout
- Progress indicator
- Auto-save draft
- Preview before publish

### Image Upload
- Drag & drop zone
- Progress indicator
- Preview with crop option
- Reorder by dragging

---

## Error Handling

| Error | Message |
|-------|---------|
| Duplicate SKU | "SKU already exists" |
| Invalid HSN | "Enter valid 4-8 digit HSN code" |
| Image too large | "Image must be under 2MB" |
| Selling > MRP | "Selling price cannot exceed MRP" |

---

## Checklist

### Development
- [ ] Create product list page
- [ ] Implement search and filters
- [ ] Build pagination
- [ ] Create add/edit product form
- [ ] Implement all form tabs
- [ ] Add image upload with preview
- [ ] Implement bulk pricing table
- [ ] Add stock update modal
- [ ] Create duplicate functionality
- [ ] Implement bulk actions
- [ ] Build CSV upload feature
- [ ] Add validation for all fields
- [ ] Connect all APIs

### Testing
- [ ] Create product works
- [ ] Edit product works
- [ ] Delete product works
- [ ] Search works
- [ ] Filters work
- [ ] Pagination works
- [ ] Image upload works
- [ ] Stock update works
- [ ] Bulk pricing calculates correctly
- [ ] CSV upload works
- [ ] Validation messages display
- [ ] Duplicate product works

---

## Dependencies
- Image upload service
- CSV parser library
- Rich text editor (optional)
- Form validation library
- Confirmation dialogs
- Toast notifications
