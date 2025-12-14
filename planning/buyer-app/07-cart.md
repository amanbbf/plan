# Buyer App - Cart Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Shopping Cart |
| Route | `/cart` or `CartScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
View and manage cart items, update quantities, see totals with tax breakup, and proceed to checkout.

---

## UI Components

### 1. Header
- Back button
- Title: "Cart (X items)"
- Clear Cart button (if items exist)

### 2. Cart Items List
Each item shows:
```
┌─────────────────────────────────────┐
│ [Img] │ Parle-G 800g                │
│       │ ₹40 x 24 packs = ₹960       │
│       │ [  -  ]  24  [  +  ]  [🗑]  │
│       │ ✓ Bulk discount applied: 5% │
└─────────────────────────────────────┘
```

### 3. Empty Cart
- Empty cart illustration
- "Your cart is empty"
- "Start Shopping" button

### 4. Cart Summary
```
┌─────────────────────────────────┐
│ Subtotal          │    ₹5,000  │
│ CGST (2.5%)       │      ₹125  │
│ SGST (2.5%)       │      ₹125  │
│ Delivery          │       Free │
│ ─────────────────────────────  │
│ Total             │    ₹5,250  │
│                                │
│ You're saving ₹500 on this order!
└─────────────────────────────────┘
```

### 5. Delivery Check
- Pin code input
- "Check" button
- Result: "Delivery available in 2-3 days"

### 6. Proceed Button (Fixed Bottom)
```
┌────────────────────────────────────┐
│  ₹5,250 (5 items)   [Proceed to Checkout]
└────────────────────────────────────┘
```

---

## Cart Item Component

### Display Elements
- Product image (thumbnail)
- Product name
- Unit price × quantity = line total
- Quantity selector
- Remove button
- Bulk discount badge (if applicable)
- Out of stock warning (if stock changed)

### Quantity Controls
- Same logic as PDP
- Respects MOQ and increment
- Updates total on change
- Debounce API calls

---

## Cart Data Structure

```json
{
  "items": [
    {
      "id": "cart_item_1",
      "product": {
        "id": "prod_123",
        "name": "Parle-G 800g",
        "image": "url",
        "price": 42,
        "mrp": 50,
        "gstRate": 5,
        "moq": 12,
        "increment": 6,
        "stock": 500,
        "inStock": true
      },
      "quantity": 24,
      "appliedPrice": 40,
      "bulkDiscount": 5,
      "lineTotal": 960
    }
  ],
  "summary": {
    "itemCount": 5,
    "subtotal": 5000,
    "taxBreakup": {
      "cgst": 125,
      "sgst": 125,
      "igst": 0
    },
    "deliveryCharge": 0,
    "discount": 0,
    "total": 5250,
    "savings": 500
  },
  "serviceability": {
    "pincode": "560001",
    "serviceable": true,
    "estimatedDays": "2-3"
  }
}
```

---

## API Calls

### GET `/api/cart`
Get current cart.

### PUT `/api/cart/items/:itemId`
Update item quantity.
```json
{
  "quantity": 36
}
```

### DELETE `/api/cart/items/:itemId`
Remove item from cart.

### DELETE `/api/cart`
Clear entire cart.

### POST `/api/cart/check-serviceability`
Check delivery serviceability.
```json
{
  "pincode": "560001"
}
```

---

## State Management

### Local State
- `cart` - Cart data object
- `isLoading` - Loading state
- `isUpdating` - Item update in progress
- `pincode` - Entered pin code
- `serviceability` - Serviceability result

### Cart Item State
- `isUpdating` - Per-item update state
- `error` - Per-item error

---

## Update Quantity Flow

1. User changes quantity
2. Show loading on item
3. Debounce (300ms)
4. Call update API
5. On success: Update cart totals
6. On error: Revert quantity, show error

---

## Remove Item Flow

1. User taps delete
2. Show confirmation: "Remove from cart?"
3. On confirm: Call delete API
4. On success: Remove item, update totals
5. Show toast: "Item removed"
6. Undo option (optional)

---

## Clear Cart Flow

1. User taps "Clear Cart"
2. Confirm: "Remove all items from cart?"
3. On confirm: Clear cart
4. Show empty cart state

---

## Stock Validation

On cart load:
1. Check current stock for each item
2. If stock < cart quantity:
   - Show warning: "Only X available"
   - Offer to adjust quantity
3. If out of stock:
   - Show "Out of Stock" badge
   - Disable item
   - "Remove" button prominent

---

## Minimum Order Value

If configured:
```
┌─────────────────────────────────┐
│ ⚠ Minimum order ₹500 required  │
│ Add ₹120 more to proceed        │
└─────────────────────────────────┘
```
- Disable Proceed button until met
- Show progress bar (optional)

---

## Serviceability Check

### Before Checkout
1. Show pin code input (pre-filled from last address)
2. User enters/confirms pin code
3. Check serviceability
4. Show result:
   - ✅ "Delivery in 2-3 days" (green)
   - ❌ "Not serviceable" (red)

### If Not Serviceable
- Show clear message
- Suggest changing address
- Cannot proceed to checkout

---

## Free Delivery Prompt

If close to free delivery threshold:
```
Add ₹200 more for FREE delivery!
[Browse Products]
```

---

## Tax Breakup

### Same State (CGST + SGST)
```
CGST (2.5%): ₹125
SGST (2.5%): ₹125
```

### Different State (IGST)
```
IGST (5%): ₹250
```

### Multiple GST Rates
- Calculate per line item
- Sum up by tax rate

---

## Design Specifications

### Layout
- Header: Fixed 56px
- Items list: Scrollable
- Summary: At bottom of list
- Proceed bar: Fixed 60px

### Cart Item Card
- Horizontal layout
- Image: 80x80px
- Full width
- Swipe to delete (optional)
- 16px padding

### Summary Card
- Distinct background
- Row items aligned
- Total row: Bold, larger font
- Savings: Green text

### Proceed Bar
- Sticky bottom
- Total on left
- Button on right
- Full width on small screens

---

## Empty States

### Empty Cart
- Illustration
- "Your cart is empty"
- "Browse products to add items"
- "Start Shopping" button

### Network Error
- "Unable to load cart"
- "Retry" button

---

## Checklist

### Development
- [ ] Create cart screen
- [ ] Fetch cart data
- [ ] Display cart items
- [ ] Build quantity selector
- [ ] Implement update quantity
- [ ] Add remove item
- [ ] Add clear cart
- [ ] Calculate and show totals
- [ ] Show tax breakup
- [ ] Add serviceability check
- [ ] Handle min order value
- [ ] Show free delivery prompt
- [ ] Stock validation
- [ ] Empty cart state
- [ ] Loading states
- [ ] Fixed proceed bar
- [ ] Navigate to checkout

### Testing
- [ ] Cart loads correctly
- [ ] Quantity update works
- [ ] Remove item works
- [ ] Clear cart works
- [ ] Totals calculate correctly
- [ ] Tax breakup accurate
- [ ] Serviceability check works
- [ ] Min order enforced
- [ ] Stock warnings show
- [ ] Proceed to checkout works

---

## Dependencies
- Cart API
- Cart state management
- Serviceability API
- Confirmation dialogs
- Toast notifications
- Tax calculation utility
