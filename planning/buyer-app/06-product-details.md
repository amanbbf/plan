# Buyer App - Product Details Page (PDP)

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Product Details |
| Route | `/product/:id` or `ProductDetailScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Show complete product information including images, pricing, bulk discounts, and allow adding to cart with quantity selection.

---

## UI Components

### 1. Header
- Back button
- Product name (truncated)
- Share button
- Cart icon with badge

### 2. Image Gallery
- Main product image (large)
- Thumbnail strip (if multiple images)
- Swipe to navigate
- Tap to zoom/fullscreen

### 3. Product Info Section
| Field | Display |
|-------|---------|
| Product Name | Large, bold |
| Brand | If available |
| SKU | Small, gray |
| Rating | Stars (if available) |

### 4. Pricing Section
```
₹42 per pack
MRP ₹50 (16% off)
GST 5% included
```

### 5. Bulk Pricing Table (if applicable)
```
┌─────────────────────────────────┐
│ Quantity     │ Price   │ Save  │
├─────────────────────────────────┤
│ 1-23 packs   │ ₹42     │ -     │
│ 24-47 packs  │ ₹40     │ 5%    │
│ 48+ packs    │ ₹39     │ 8%    │
└─────────────────────────────────┘
```

### 6. Stock Status
- ✅ "In Stock" (green)
- ⚠️ "Low Stock - Only X left" (orange)
- ❌ "Out of Stock" (red)

### 7. Quantity Selector
```
┌──────────────────────────────┐
│  [ - ]     24     [ + ]      │
│  Min order: 12 packs         │
└──────────────────────────────┘
```

### 8. Product Description
- Collapsible/expandable
- Rich text content
- Specifications table

### 9. Additional Info
- HSN Code: XXXX
- Unit: Pack of 800g
- Weight: 0.8 kg

### 10. Add to Cart Section (Fixed Bottom)
```
┌────────────────────────────────────┐
│ Total: ₹1,008          [Add to Cart]
└────────────────────────────────────┘
```

### 11. Related Products
- Horizontal scroll
- "You might also like"
- Similar products

---

## Product Data

```json
{
  "id": "prod_123",
  "name": "Parle-G Biscuit Family Pack 800g",
  "brand": "Parle",
  "sku": "BIS-PG-800",
  "images": [
    { "url": "...", "isPrimary": true },
    { "url": "...", "isPrimary": false }
  ],
  "description": "Family pack glucose biscuits...",
  "mrp": 50,
  "price": 42,
  "discount": 16,
  "gstRate": 5,
  "hsnCode": "1905",
  "moq": 12,
  "increment": 6,
  "maxQty": 100,
  "unit": "pack",
  "weight": 0.8,
  "stock": 500,
  "inStock": true,
  "bulkPricing": [
    { "minQty": 1, "maxQty": 23, "price": 42 },
    { "minQty": 24, "maxQty": 47, "price": 40 },
    { "minQty": 48, "maxQty": null, "price": 39 }
  ],
  "category": { "id": "cat_1", "name": "Snacks" },
  "subcategory": { "id": "subcat_1", "name": "Biscuits" },
  "relatedProducts": [...]
}
```

---

## API Calls

### GET `/api/products/:id`
Get product details.

### POST `/api/cart/add`
Add product to cart.

```json
{
  "productId": "prod_123",
  "quantity": 24
}
```

---

## State Management

### Local State
- `product` - Product details
- `quantity` - Selected quantity
- `selectedImage` - Current image index
- `isLoading` - Loading state
- `isAddingToCart` - Add button loading

### Computed Values
- `currentPrice` - Based on quantity (bulk pricing)
- `totalPrice` - quantity × currentPrice
- `savings` - mrp × quantity - totalPrice

---

## Quantity Selector Logic

### Rules
1. Start at MOQ
2. Minimum = MOQ
3. Maximum = maxQty or stock (whichever lower)
4. Increment by `increment` value
5. Cannot go below MOQ

### Example
```
MOQ: 12, Increment: 6
Valid quantities: 12, 18, 24, 30, 36...
```

### Button States
- "-" disabled at MOQ
- "+" disabled at max
- Direct input allowed (rounds to nearest valid)

---

## Bulk Pricing Calculation

When quantity changes:
1. Find applicable tier
2. Update displayed price
3. Show savings if applicable
4. Update total

### Display
```
Buying 24 packs @ ₹40/pack = ₹960
You save ₹48 (5% off)
```

---

## Add to Cart Flow

1. Validate quantity (>= MOQ, <= max)
2. Call API to add to cart
3. Show loading on button
4. On success: Toast "Added to cart" with "View Cart" action
5. Update cart badge
6. On error: Show error message

### If Already in Cart
- Button shows "Update Cart" or "Go to Cart"
- Quantity selector shows current cart quantity

---

## Share Functionality

Share product:
- Product name
- Image (if supported)
- Deep link to product

---

## Out of Stock State

When out of stock:
- Disable quantity selector
- Disable Add to Cart button
- Show "Out of Stock" prominently
- Optional: "Notify when available" button

---

## Design Specifications

### Layout
- Full screen, scrollable
- Image: 60% viewport height
- Fixed bottom bar for Add to Cart

### Image Gallery
- Swipeable
- Dot indicators
- Pinch to zoom (fullscreen)

### Pricing
- Current price: 24px, bold, dark
- MRP: 16px, strikethrough, gray
- Discount badge: Green background

### Quantity Selector
- Large touch targets (48px)
- Number input editable
- Clear +/- buttons

### Bulk Pricing Table
- Striped rows
- Current tier highlighted
- Clear column headers

### Bottom Bar
- Fixed position
- Shadow/border top
- 60px height
- Total on left, button on right

---

## Checklist

### Development
- [ ] Create product detail screen
- [ ] Implement image gallery
- [ ] Add image zoom functionality
- [ ] Display product info
- [ ] Show pricing with discount
- [ ] Create bulk pricing table
- [ ] Build quantity selector
- [ ] Implement MOQ/increment logic
- [ ] Calculate dynamic pricing
- [ ] Show stock status
- [ ] Fixed bottom Add to Cart bar
- [ ] Add to cart functionality
- [ ] Cart badge update
- [ ] Share button
- [ ] Related products section
- [ ] Handle out of stock
- [ ] Loading and error states

### Testing
- [ ] Product loads correctly
- [ ] Images display and swipe
- [ ] Quantity selector works
- [ ] MOQ enforced
- [ ] Bulk pricing calculates
- [ ] Total updates correctly
- [ ] Add to cart works
- [ ] Out of stock handled
- [ ] Share works

---

## Dependencies
- Product API
- Cart state management
- Image gallery/zoom library
- Share API
- Toast notifications
