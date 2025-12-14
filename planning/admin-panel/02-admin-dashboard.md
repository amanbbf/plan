# Admin Panel - Dashboard Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Admin Dashboard |
| URL | `/admin/dashboard` |
| Access | Authenticated (All admin roles) |
| Priority | P0 - Critical |
| Responsive | Desktop only |

---

## Purpose
Central hub showing key business metrics, recent activity, and quick actions for admin users.

---

## UI Components

### 1. Header
- Page title: "Dashboard"
- Welcome message: "Welcome back, [Admin Name]"
- Date/time display

### 2. Stats Cards Row
| Card | Value | Icon | Color |
|------|-------|------|-------|
| Today's Orders | Count | 📦 | Blue |
| Today's Revenue | ₹ Amount | 💰 | Green |
| Pending Orders | Count | ⏳ | Orange |
| Low Stock Items | Count | ⚠️ | Red |

### 3. Charts Section (2 columns)

**Chart 1: Sales Trend (Line Chart)**
- Last 7 days or 30 days toggle
- X-axis: Dates
- Y-axis: Revenue in ₹

**Chart 2: Order Status (Pie/Donut Chart)**
- Pending
- Confirmed
- Shipped
- Delivered
- Cancelled

### 4. Recent Orders Table
| Column | Description |
|--------|-------------|
| Order ID | Clickable link to order details |
| Buyer Name | Business name |
| Amount | Order total |
| Status | Badge with color |
| Date | Order date |
| Action | View button |

- Show last 10 orders
- "View All Orders" link

### 5. Pending Approvals Widget
- Count of pending buyer registrations
- Quick link to approval page

### 6. Quick Actions
- Add New Product
- Create Order
- View All Orders
- Manage Stock

---

## Data Requirements

### Stats Data
```json
{
  "todayOrders": 15,
  "todayRevenue": 45000,
  "pendingOrders": 8,
  "lowStockItems": 5
}
```

### Sales Trend Data
```json
{
  "period": "7days",
  "data": [
    { "date": "2024-01-01", "revenue": 25000 },
    { "date": "2024-01-02", "revenue": 32000 },
    ...
  ]
}
```

### Order Status Distribution
```json
{
  "pending": 10,
  "confirmed": 5,
  "shipped": 8,
  "delivered": 45,
  "cancelled": 2
}
```

---

## API Calls

### GET `/api/admin/dashboard/stats`
Returns today's key metrics.

### GET `/api/admin/dashboard/sales-trend?period=7days`
Returns sales data for chart.

### GET `/api/admin/dashboard/order-status`
Returns order status distribution.

### GET `/api/admin/orders?limit=10&sort=createdAt:desc`
Returns recent orders for table.

### GET `/api/admin/buyers/pending/count`
Returns count of pending approvals.

---

## State Management

### Local State
- `stats` - Dashboard stats object
- `salesTrend` - Array of daily revenue
- `orderStatus` - Status distribution object
- `recentOrders` - Array of order objects
- `pendingApprovals` - Count
- `isLoading` - Loading states for each section
- `period` - Selected period for chart (7days/30days)

---

## Role-Based Visibility

| Widget | Super Admin | Operations | Finance |
|--------|-------------|------------|---------|
| Stats Cards | All | All | Revenue only |
| Sales Chart | ✅ | ✅ | ✅ |
| Order Status | ✅ | ✅ | ❌ |
| Recent Orders | ✅ | ✅ | View only |
| Pending Approvals | ✅ | ✅ | ❌ |
| Quick Actions | All | Limited | Limited |

---

## User Interactions

| Action | Result |
|--------|--------|
| Click stat card | Navigate to related page |
| Toggle chart period | Reload chart with new data |
| Click order row | Navigate to order details |
| Click quick action | Navigate to respective page |
| Click pending approvals | Navigate to buyer approvals |

---

## Refresh Behavior
- Auto-refresh every 5 minutes
- Manual refresh button available
- Loading skeleton during refresh

---

## Design Specifications

### Layout
- Full width container
- Stats: 4-column grid
- Charts: 2-column grid
- Recent orders: Full width table

### Spacing
- Section gap: 24px
- Card padding: 20px
- Table row height: 48px

### Colors
- Stats card backgrounds: Light variants of icon colors
- Chart colors: Consistent palette
- Status badges: Semantic colors

---

## Empty States

| Scenario | Message |
|----------|---------|
| No orders today | "No orders placed today" |
| No data for chart | "Not enough data for chart" |
| No pending approvals | Badge shows "0" |

---

## Error Handling

| Error | Behavior |
|-------|----------|
| API failure | Show error toast, retry button |
| Partial failure | Show available data, error for failed sections |
| Network error | Show offline indicator |

---

## Checklist

### Development
- [ ] Create dashboard layout
- [ ] Implement stats cards
- [ ] Add sales trend chart
- [ ] Add order status chart
- [ ] Create recent orders table
- [ ] Add pending approvals widget
- [ ] Implement quick actions
- [ ] Add period toggle for chart
- [ ] Implement auto-refresh
- [ ] Add role-based visibility

### Testing
- [ ] Stats display correctly
- [ ] Charts render properly
- [ ] Table pagination works
- [ ] Navigation works
- [ ] Role-based access enforced
- [ ] Auto-refresh works
- [ ] Error states handled

---

## Dependencies
- Chart library (Chart.js or similar)
- Date formatting utility
- Currency formatting utility
- Role-based access control
- Multiple dashboard APIs
