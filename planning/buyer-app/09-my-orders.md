# Buyer App - My Orders Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | My Orders |
| Route | `/orders` or `OrdersScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
View order history, track current orders, download invoices, and reorder previous orders.

---

## UI Components

### 1. Header
- Back button (or from bottom nav)
- Title: "My Orders"
- Search icon (search orders)

### 2. Order Status Tabs
```
[All] [Active] [Delivered] [Cancelled]
```

### 3. Order Cards List
```
┌─────────────────────────────────────┐
│ Order #ORD-2024-0001                │
│ 15 Jan 2024 • 5 items               │
│                                     │
│ ₹5,250                              │
│ ● Shipped                           │
│                                     │
│ [Track Order]  [View Details]       │
└─────────────────────────────────────┘
```

### 4. Empty State
- No orders illustration
- "No orders yet"
- "Start Shopping" button

---

## Order Card Component

### Display Elements
- Order number
- Order date
- Item count
- Total amount
- Status badge
- Quick actions

### Status Badge Colors
| Status | Color | Label |
|--------|-------|-------|
| pending | Yellow | Pending |
| confirmed | Blue | Confirmed |
| packed | Purple | Packed |
| shipped | Indigo | Shipped |
| in_transit | Cyan | In Transit |
| delivered | Green | Delivered |
| cancelled | Red | Cancelled |

### Quick Actions (based on status)
| Status | Actions |
|--------|---------|
| pending | View Details, Cancel |
| confirmed/packed | Track, View Details |
| shipped | Track, View Details |
| delivered | Reorder, Invoice, Details |
| cancelled | Reorder, Details |

---

## API Calls

### GET `/api/orders`
List buyer's orders.

**Query Params:**
- `status` - Filter by status
- `page`, `limit` - Pagination
- `search` - Search order number

**Response:**
```json
{
  "success": true,
  "data": {
    "orders": [
      {
        "id": "order_123",
        "orderNumber": "ORD-2024-0001",
        "createdAt": "2024-01-15T10:00:00Z",
        "itemCount": 5,
        "total": 5250,
        "status": "shipped",
        "shipping": {
          "courier": "India Post",
          "awbNumber": "EE123456789IN",
          "estimatedDelivery": "2024-01-20"
        }
      }
    ],
    "pagination": {
      "total": 25,
      "page": 1,
      "limit": 10
    }
  }
}
```

---

## State Management

### Local State
- `orders` - Array of orders
- `activeTab` - Current status filter
- `pagination` - Pagination state
- `isLoading` - Loading state
- `searchQuery` - Search input

---

## Tab Filtering

| Tab | Filter |
|-----|--------|
| All | No filter |
| Active | pending, confirmed, packed, shipped, in_transit |
| Delivered | delivered |
| Cancelled | cancelled |

---

## Pagination

- Load 10 orders per page
- Infinite scroll or Load More button
- Show loading indicator

---

## Search Orders

- Search by order number
- Debounced search
- Show "No results" for empty

---

## Reorder Flow

1. User taps "Reorder"
2. Add all items from that order to cart
   - Check stock availability
   - Use current prices
3. Navigate to cart
4. Show items added confirmation

### Out of Stock Handling
- Skip unavailable items
- Show message: "X items were unavailable"
- Add available items only

---

## Empty States

### No Orders
- Illustration
- "You haven't placed any orders yet"
- "Start Shopping" button

### No Active Orders
- "No active orders"
- "Browse Products" link

### No Search Results
- "No orders found for '[query]'"
- "Try different keywords"

---

## Design Specifications

### Layout
- Tabs at top (sticky)
- Order cards list
- Scroll with pagination

### Order Card
- Full width
- 16px padding
- Subtle border/shadow
- Status badge right-aligned
- Actions at bottom

### Tabs
- Horizontal scroll if needed
- Active tab highlighted
- Count badges (optional)

---

## Checklist

### Development
- [ ] Create orders list screen
- [ ] Implement status tabs
- [ ] Create order card component
- [ ] Load orders from API
- [ ] Implement pagination
- [ ] Add search functionality
- [ ] Implement reorder
- [ ] Add empty states
- [ ] Navigation to order details
- [ ] Loading states

### Testing
- [ ] Orders load correctly
- [ ] Tabs filter correctly
- [ ] Pagination works
- [ ] Search works
- [ ] Reorder works
- [ ] Status badges correct
- [ ] Navigation works

---

## Dependencies
- Orders API
- Cart state (for reorder)
- Navigation library
- Search/filter utilities
