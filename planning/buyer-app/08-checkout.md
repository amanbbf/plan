# Buyer App - Checkout Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Checkout |
| Route | `/checkout` or `CheckoutScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Complete the order by selecting delivery address, payment method, and placing the order.

---

## Checkout Steps

```
[1] Address ─── [2] Payment ─── [3] Review ─── [4] Confirm
```

---

## UI Components

### 1. Header
- Back button
- Title: "Checkout"
- Step indicator

### 2. Step 1: Delivery Address

**Saved Addresses**
```
┌─────────────────────────────────────┐
│ ● Krishna Stores (Default)          │
│   123 Main Road, Near Bus Stand     │
│   Bangalore, Karnataka 560001       │
│   Phone: 9876543210                 │
│   [Edit] [Remove]                   │
├─────────────────────────────────────┤
│ ○ Warehouse Address                 │
│   456 Industrial Area               │
│   Bangalore, Karnataka 560002       │
└─────────────────────────────────────┘
```

**Add New Address Button**
- Opens address form

**Serviceability Check**
- Auto-check selected address pin code
- Show delivery estimate

### 3. Step 2: Payment Method

**Payment Options**
```
┌─────────────────────────────────────┐
│ ● UPI Payment                       │
│   Pay online using any UPI app      │
├─────────────────────────────────────┤
│ ○ Cash on Delivery (COD)            │
│   Pay when you receive the order    │
├─────────────────────────────────────┤
│ ○ Credit (Due: 15 days)             │
│   Available credit: ₹5,000          │
│   Order amount: ₹5,250              │
│   ⚠ Insufficient credit             │
└─────────────────────────────────────┘
```

### 4. Step 3: Order Review

**Order Summary**
- List of items with quantities
- Price breakup
- Delivery address
- Payment method
- Estimated delivery

**Special Instructions**
- Text input for order notes

### 5. Step 4: Place Order (Fixed Bottom)
```
┌────────────────────────────────────┐
│  ₹5,250              [Place Order] │
└────────────────────────────────────┘
```

---

## Address Form (Add/Edit)

| Field | Type | Required |
|-------|------|----------|
| Address Label | Text (e.g., "Shop", "Warehouse") | Yes |
| Contact Name | Text | Yes |
| Phone | Number | Yes |
| Address Line 1 | Text | Yes |
| Address Line 2 | Text | No |
| City | Text | Yes |
| State | Dropdown | Yes |
| Pin Code | Number (6 digits) | Yes |
| Landmark | Text | No |
| Set as Default | Toggle | No |

---

## Payment Methods

### UPI
- Online payment option
- Redirects to payment gateway
- Auto-confirmation on success

### COD (Cash on Delivery)
- Pay at delivery time
- May have additional charge (configurable)
- Subject to availability

### Credit
- Use available credit limit
- Shows available balance
- Disabled if insufficient credit
- Shows payment due date

---

## Order Data Structure

```json
{
  "items": [
    {
      "productId": "prod_123",
      "name": "Parle-G 800g",
      "quantity": 24,
      "price": 40,
      "total": 960
    }
  ],
  "address": {
    "id": "addr_1",
    "label": "Krishna Stores",
    "name": "Ramesh Kumar",
    "phone": "9876543210",
    "line1": "123 Main Road",
    "line2": "Near Bus Stand",
    "city": "Bangalore",
    "state": "Karnataka",
    "pincode": "560001"
  },
  "payment": {
    "method": "upi",
    "status": "pending"
  },
  "pricing": {
    "subtotal": 5000,
    "cgst": 125,
    "sgst": 125,
    "deliveryCharge": 0,
    "total": 5250
  },
  "notes": "Please deliver before 5 PM",
  "estimatedDelivery": "2-3 days"
}
```

---

## API Calls

### GET `/api/addresses`
Get saved addresses.

### POST `/api/addresses`
Add new address.

### PUT `/api/addresses/:id`
Update address.

### DELETE `/api/addresses/:id`
Delete address.

### POST `/api/orders`
Create order.

**Request:**
```json
{
  "addressId": "addr_1",
  "paymentMethod": "upi",
  "notes": "Deliver before 5 PM"
}
```

**Response:**
```json
{
  "success": true,
  "order": {
    "id": "order_123",
    "orderNumber": "ORD-2024-0001",
    "total": 5250,
    "status": "pending"
  },
  "payment": {
    "method": "upi",
    "redirectUrl": "https://pay.example.com/..."
  }
}
```

---

## State Management

### Local State
- `step` - Current checkout step
- `addresses` - Saved addresses
- `selectedAddress` - Selected address ID
- `paymentMethod` - Selected payment
- `notes` - Order notes
- `isLoading` - Loading state
- `isPlacingOrder` - Order submission state

---

## Checkout Flow

```
1. Load saved addresses
2. Select/add delivery address
3. Check serviceability
4. Select payment method
5. Review order summary
6. Add notes (optional)
7. Place order
   ├── UPI → Redirect to payment → Return → Show success
   ├── COD → Create order → Show success
   └── Credit → Create order → Show success
8. Navigate to order confirmation
```

---

## UPI Payment Flow

1. User selects UPI
2. User clicks "Place Order"
3. Order created with status "pending_payment"
4. Redirect to payment gateway
5. User completes payment
6. Callback to app/web
7. Verify payment status
8. Show order confirmation

### Payment Failure
- Order remains pending
- User can retry payment
- Auto-cancel after timeout (30 mins)

---

## Credit Payment Flow

1. User selects Credit
2. Check available credit >= order total
3. If insufficient: Show warning, disable option
4. If sufficient: Allow selection
5. On order: Deduct from credit
6. Create ledger entry
7. Set payment due date

---

## Validation Rules

### Before Proceeding
- Address selected
- Address is serviceable
- Payment method selected
- For Credit: Sufficient balance
- All cart items in stock

### Address
- All required fields filled
- Valid pin code format
- Pin code is serviceable

---

## Error Handling

| Error | Message |
|-------|---------|
| No addresses | "Please add a delivery address" |
| Not serviceable | "We don't deliver to this pin code" |
| Insufficient credit | "Insufficient credit. Choose another payment method." |
| Out of stock | "Some items are out of stock" |
| Payment failed | "Payment failed. Please try again." |
| Order failed | "Unable to place order. Please try again." |

---

## Order Success Screen

```
┌─────────────────────────────────────┐
│           ✓                         │
│   Order Placed Successfully!        │
│                                     │
│   Order #ORD-2024-0001              │
│   ₹5,250                            │
│                                     │
│   Estimated delivery: 2-3 days      │
│                                     │
│   [Track Order]  [Continue Shopping]│
└─────────────────────────────────────┘
```

---

## Design Specifications

### Layout
- Header with steps
- Content scrollable
- Fixed bottom bar

### Steps Indicator
- Horizontal dots/numbers
- Current step highlighted
- Completed steps with checkmark

### Address Cards
- Radio selection
- Full width
- Edit/delete options

### Payment Options
- Radio selection
- Icons for each method
- Disabled state for unavailable

### Review Section
- Collapsible item list
- Clear price breakup
- Editable notes field

---

## Checklist

### Development
- [ ] Create checkout flow
- [ ] Implement step navigation
- [ ] Load saved addresses
- [ ] Build address selector
- [ ] Create add address form
- [ ] Implement serviceability check
- [ ] Build payment method selector
- [ ] Handle UPI payment flow
- [ ] Handle COD selection
- [ ] Handle credit payment
- [ ] Create order review
- [ ] Add special instructions input
- [ ] Implement place order
- [ ] Handle payment gateway
- [ ] Create success screen
- [ ] Handle all errors
- [ ] Clear cart on success

### Testing
- [ ] Address selection works
- [ ] Add address works
- [ ] Serviceability check works
- [ ] Payment selection works
- [ ] UPI flow works
- [ ] COD flow works
- [ ] Credit flow works
- [ ] Order creation works
- [ ] Success screen shows
- [ ] Cart cleared after order

---

## Dependencies
- Address API
- Order API
- Payment gateway SDK
- Cart state
- Credit check API
- Serviceability API
