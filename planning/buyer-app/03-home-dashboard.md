# Buyer App - Home / Dashboard Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Home / Dashboard |
| Route | `/home` or `HomeScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Central landing page showing featured products, categories, quick actions, and personalized content for the buyer.

---

## UI Components

### 1. Header
- App logo
- Search bar (tap to open search)
- Cart icon with badge (item count)
- Notifications icon with badge

### 2. Welcome Banner
- "Welcome, [Business Name]!"
- Quick stats or greeting

### 3. Categories Grid
- Category icons in grid (2x3 or scrollable)
- Category name below icon
- Tap to open category page
- "View All" link

### 4. Featured Products Section
- Horizontal scrollable list
- Product cards with image, name, price, MOQ
- "Add to Cart" button
- "View All" link

### 5. Offers/Promotions Banner
- Carousel/slider
- Promotional images
- Tap to view offer details

### 6. Recent Orders Widget
- Last 2-3 orders summary
- Order ID, date, status
- "View All Orders" link

### 7. Quick Actions
- Repeat Last Order
- View Cart
- My Orders
- Support/Help

### 8. Bottom Navigation
- Home (active)
- Categories
- Cart
- Orders
- Profile

---

## Data Requirements

### User Info
```json
{
  "businessName": "Krishna Stores",
  "creditAvailable": 5000,
  "pendingOrders": 2
}
```

### Categories
```json
[
  { "id": "cat_1", "name": "Beverages", "icon": "url", "productCount": 45 },
  { "id": "cat_2", "name": "Snacks", "icon": "url", "productCount": 78 }
]
```

### Featured Products
```json
[
  {
    "id": "prod_1",
    "name": "Parle-G 800g",
    "image": "url",
    "mrp": 50,
    "price": 42,
    "moq": 12,
    "unit": "pack"
  }
]
```

### Recent Orders
```json
[
  {
    "id": "order_1",
    "orderNumber": "ORD-2024-001",
    "date": "2024-01-15",
    "total": 5200,
    "status": "shipped"
  }
]
```

---

## API Calls

### GET `/api/home`
Aggregated home page data.

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {...},
    "categories": [...],
    "featuredProducts": [...],
    "banners": [...],
    "recentOrders": [...]
  }
}
```

### GET `/api/categories`
All categories for grid.

### GET `/api/products/featured`
Featured products list.

### GET `/api/cart/count`
Cart item count for badge.

### GET `/api/notifications/unread-count`
Notification badge count.

---

## State Management

### Local State
- `homeData` - Aggregated home data
- `isLoading` - Loading state
- `isRefreshing` - Pull-to-refresh state
- `cartCount` - Cart badge count
- `notificationCount` - Notification badge

### Global State
- User authentication
- Cart state
- Notification state

---

## User Interactions

| Action | Result |
|--------|--------|
| Tap search bar | Open search screen |
| Tap cart icon | Navigate to cart |
| Tap notification icon | Open notifications |
| Tap category | Navigate to category products |
| Tap "View All" categories | Navigate to all categories |
| Tap product card | Navigate to product details |
| Tap "Add to Cart" | Add MOQ to cart, show confirmation |
| Tap banner | Navigate to offer/collection |
| Tap order | Navigate to order details |
| Pull down | Refresh home data |
| Tap bottom nav | Navigate to respective screen |

---

## Add to Cart (Quick Add)

When tapping "Add to Cart" on home:
1. Add MOQ quantity to cart
2. Show toast: "Added [Product] to cart"
3. Update cart badge
4. Show "Go to Cart" action in toast

---

## Pull to Refresh

- Pull down gesture
- Show refresh indicator
- Reload all home data
- Update cart/notification counts

---

## Credit Display (Optional)

If buyer has credit:
- Show available credit: "Credit: ₹5,000"
- Color code: Green if available, Red if low

---

## Empty States

### No Featured Products
"New products coming soon!"

### No Recent Orders
"You haven't placed any orders yet. Start shopping!"

### Network Error
"Unable to load. Pull down to retry."

---

## Design Specifications

### Layout (Mobile)
- Header: 60px fixed
- Content: Scrollable
- Bottom nav: 60px fixed
- Safe area handling

### Categories Grid
- 2 columns on mobile
- 3-4 columns on tablet
- Equal aspect ratio icons
- Text below, centered

### Product Cards
- Horizontal scroll
- Card width: ~160px
- Image: Square, 120px
- Name: 2 lines max
- Price prominent
- Small "Add" button

### Banner Carousel
- Full width
- 3:1 or 16:9 aspect ratio
- Auto-scroll (5 seconds)
- Dot indicators

### Colors
- Category icons: Colorful backgrounds
- Product cards: White with shadow
- Prices: Green (selling), Gray strikethrough (MRP)

---

## Accessibility

- [ ] Screen reader labels for icons
- [ ] Alt text for images
- [ ] Touch targets 44px minimum
- [ ] Color contrast compliant
- [ ] Skip navigation option

---

## Performance

- Lazy load images
- Cache home data
- Skeleton loading
- Optimistic cart updates

---

## Checklist

### Development
- [ ] Create home screen layout
- [ ] Implement header with search/cart/notification
- [ ] Add categories grid
- [ ] Create product card component
- [ ] Implement featured products scroll
- [ ] Add banner carousel
- [ ] Create recent orders widget
- [ ] Implement quick actions
- [ ] Add bottom navigation
- [ ] Implement pull-to-refresh
- [ ] Add quick add to cart
- [ ] Handle loading states
- [ ] Handle empty states
- [ ] Handle errors

### Testing
- [ ] Home loads correctly
- [ ] Categories display properly
- [ ] Products display correctly
- [ ] Banner carousel works
- [ ] Cart badge updates
- [ ] Navigation works
- [ ] Pull to refresh works
- [ ] Add to cart works

---

## Dependencies
- Navigation library
- Image caching
- Carousel component
- Toast notifications
- Cart state management
- API aggregation endpoint
