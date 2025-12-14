# Backend API Endpoints

## Overview

RESTful API design for the B2B Grocery Wholesale Platform.

**Base URL:** `/api/v1`
**Authentication:** JWT Bearer Token
**Content Type:** `application/json`

---

## API Structure

```
/api/v1
├── /auth           # Authentication
├── /admin          # Admin panel APIs
│   ├── /dashboard
│   ├── /buyers
│   ├── /categories
│   ├── /subcategories
│   ├── /products
│   ├── /orders
│   ├── /inventory
│   ├── /invoices
│   ├── /payments
│   ├── /pincodes
│   ├── /couriers
│   ├── /settings
│   └── /users
└── /buyer          # Buyer app APIs
    ├── /profile
    ├── /addresses
    ├── /categories
    ├── /products
    ├── /cart
    ├── /orders
    ├── /notifications
    └── /credit
```

---

## Authentication APIs

### Public Endpoints

#### POST `/auth/send-otp`
Send OTP to phone number.

**Request:**
```json
{
  "phone": "9876543210",
  "purpose": "login" | "register"
}
```

**Response:**
```json
{
  "success": true,
  "message": "OTP sent successfully",
  "expiresIn": 300
}
```

---

#### POST `/auth/verify-otp`
Verify OTP.

**Request:**
```json
{
  "phone": "9876543210",
  "otp": "123456",
  "purpose": "login" | "register"
}
```

**Response (Login):**
```json
{
  "success": true,
  "token": "jwt_token",
  "user": {
    "id": "buyer_123",
    "businessName": "Krishna Stores",
    "status": "active"
  }
}
```

---

#### POST `/auth/register`
Register new buyer.

**Request:**
```json
{
  "phone": "9876543210",
  "business": {
    "name": "Krishna Stores",
    "type": "kirana",
    "ownerName": "Ramesh Kumar",
    "email": "ramesh@email.com",
    "gstNumber": "29AABCU9603R1ZM"
  },
  "address": {
    "line1": "123 Main Road",
    "city": "Bangalore",
    "state": "Karnataka",
    "pincode": "560001"
  }
}
```

**Response:**
```json
{
  "success": true,
  "message": "Registration successful. Pending approval.",
  "buyerId": "buyer_123"
}
```

---

#### POST `/auth/admin/login`
Admin login.

**Request:**
```json
{
  "email": "admin@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "token": "jwt_token",
  "user": {
    "id": "admin_123",
    "name": "Admin Name",
    "role": "super_admin"
  }
}
```

---

## Admin APIs

### Dashboard

#### GET `/admin/dashboard/stats`
Get dashboard statistics.

**Response:**
```json
{
  "todayOrders": 15,
  "todayRevenue": 45000,
  "pendingOrders": 8,
  "lowStockItems": 5
}
```

---

#### GET `/admin/dashboard/sales-trend`
Get sales trend data.

**Query:** `?period=7days|30days`

**Response:**
```json
{
  "period": "7days",
  "data": [
    { "date": "2024-01-15", "revenue": 25000, "orders": 12 }
  ]
}
```

---

### Buyer Management

#### GET `/admin/buyers`
List all buyers.

**Query:** `?page=1&limit=25&status=pending&search=krishna`

**Response:**
```json
{
  "buyers": [...],
  "pagination": { "total": 150, "page": 1, "pages": 6 }
}
```

---

#### GET `/admin/buyers/:id`
Get buyer details.

---

#### PUT `/admin/buyers/:id/approve`
Approve buyer registration.

**Request:**
```json
{
  "creditLimit": 10000,
  "notes": "Verified documents"
}
```

---

#### PUT `/admin/buyers/:id/reject`
Reject buyer registration.

**Request:**
```json
{
  "reason": "Invalid GST number"
}
```

---

#### PUT `/admin/buyers/:id/credit-limit`
Update credit limit.

**Request:**
```json
{
  "creditLimit": 25000
}
```

---

#### PUT `/admin/buyers/:id/block`
Block buyer account.

---

### Category Management

#### GET `/admin/categories`
List all categories with subcategories.

---

#### POST `/admin/categories`
Create category.

**Request:**
```json
{
  "name": "Beverages",
  "description": "All drinks",
  "image": "base64_or_url",
  "status": "active",
  "sortOrder": 1
}
```

---

#### PUT `/admin/categories/:id`
Update category.

---

#### DELETE `/admin/categories/:id`
Delete category (if no products).

---

### Subcategory Management

#### GET `/admin/categories/:id/subcategories`
List subcategories of a category.

---

#### POST `/admin/subcategories`
Create subcategory.

**Request:**
```json
{
  "categoryId": "cat_123",
  "name": "Cold Drinks",
  "status": "active"
}
```

---

#### PUT `/admin/subcategories/:id`
Update subcategory.

---

#### DELETE `/admin/subcategories/:id`
Delete subcategory.

---

### Product Management

#### GET `/admin/products`
List products with filters.

**Query:** `?page=1&limit=25&category=cat_123&status=active&search=parle`

---

#### GET `/admin/products/:id`
Get product details.

---

#### POST `/admin/products`
Create product.

**Request:**
```json
{
  "name": "Parle-G 800g",
  "sku": "BIS-PG-800",
  "description": "Family pack biscuits",
  "categoryId": "cat_123",
  "subcategoryId": "subcat_456",
  "brand": "Parle",
  "images": [{ "url": "...", "isPrimary": true }],
  "pricing": {
    "mrp": 50,
    "sellingPrice": 42,
    "gstRate": 5,
    "hsnCode": "1905"
  },
  "inventory": {
    "stock": 500,
    "lowStockAlert": 50,
    "unit": "pack"
  },
  "ordering": {
    "moq": 12,
    "increment": 6
  },
  "bulkPricing": [
    { "minQty": 24, "maxQty": 47, "discountPercent": 5 }
  ],
  "settings": {
    "status": "active",
    "featured": false
  }
}
```

---

#### PUT `/admin/products/:id`
Update product.

---

#### DELETE `/admin/products/:id`
Delete product (soft delete).

---

#### POST `/admin/products/:id/duplicate`
Duplicate product.

---

#### POST `/admin/products/bulk-upload`
Bulk upload via CSV.

**Request:** `multipart/form-data` with CSV file

---

### Order Management

#### GET `/admin/orders`
List orders with filters.

**Query:** `?page=1&limit=25&status=pending&startDate=2024-01-01`

---

#### GET `/admin/orders/:id`
Get order details.

---

#### PUT `/admin/orders/:id/status`
Update order status.

**Request:**
```json
{
  "status": "shipped",
  "courier": "India Post",
  "awbNumber": "EE123456789IN",
  "shippedDate": "2024-01-16",
  "estimatedDelivery": "2024-01-20",
  "note": "Speed post"
}
```

---

#### PUT `/admin/orders/:id/payment`
Update payment status.

**Request:**
```json
{
  "status": "paid",
  "amountPaid": 5250,
  "transactionId": "TXN123"
}
```

---

#### PUT `/admin/orders/:id/cancel`
Cancel order.

**Request:**
```json
{
  "reason": "Out of stock"
}
```

---

### Inventory Management

#### GET `/admin/inventory`
List products with stock info.

**Query:** `?status=lowstock&category=cat_123`

---

#### POST `/admin/inventory/:productId/adjust`
Adjust stock.

**Request:**
```json
{
  "type": "add",
  "quantity": 100,
  "reason": "purchase",
  "reference": "PO-2024-001",
  "notes": "Supplier delivery"
}
```

---

#### GET `/admin/inventory/:productId/history`
Get stock history.

---

### Invoice Management

#### GET `/admin/invoices`
List invoices.

**Query:** `?startDate=2024-01-01&endDate=2024-01-31`

---

#### GET `/admin/invoices/:id`
Get invoice details.

---

#### GET `/admin/invoices/:id/pdf`
Download invoice PDF.

---

#### POST `/admin/invoices/:id/send`
Email invoice to buyer.

---

### Payment Management

#### GET `/admin/payments`
List payments.

**Query:** `?method=upi&status=success&startDate=2024-01-01`

---

#### POST `/admin/payments`
Record manual payment.

**Request:**
```json
{
  "orderId": "order_123",
  "amount": 5250,
  "method": "cod",
  "transactionId": "CASH-001",
  "paymentDate": "2024-01-16"
}
```

---

#### GET `/admin/credit/buyers`
List buyers with credit.

---

#### GET `/admin/credit/buyers/:id/ledger`
Get credit ledger for buyer.

---

### Pincode Management

#### GET `/admin/pincodes`
List serviceable pincodes.

---

#### POST `/admin/pincodes`
Add pincode.

**Request:**
```json
{
  "pincode": "560001",
  "area": "MG Road",
  "city": "Bangalore",
  "state": "Karnataka",
  "delivery": {
    "daysMin": 1,
    "daysMax": 2,
    "charge": 0
  },
  "status": "active"
}
```

---

#### POST `/admin/pincodes/bulk-upload`
Bulk upload pincodes.

---

### Courier Management

#### GET `/admin/couriers`
List couriers.

---

#### POST `/admin/couriers`
Add courier.

**Request:**
```json
{
  "name": "India Post",
  "code": "INDPOST",
  "trackingUrlTemplate": "https://...?awb={{awb}}",
  "status": "active"
}
```

---

### Settings

#### GET `/admin/settings/:section`
Get settings for section.

**Sections:** `company`, `tax`, `ordering`, `credit`, `delivery`, `notifications`

---

#### PUT `/admin/settings/:section`
Update settings.

---

### Admin Users

#### GET `/admin/users`
List admin users.

---

#### POST `/admin/users`
Create admin user.

---

#### PUT `/admin/users/:id`
Update admin user.

---

#### DELETE `/admin/users/:id`
Delete admin user.

---

## Buyer APIs

### Home

#### GET `/buyer/home`
Get aggregated home data.

**Response:**
```json
{
  "user": { "businessName": "...", "creditAvailable": 5000 },
  "categories": [...],
  "featuredProducts": [...],
  "banners": [...],
  "recentOrders": [...]
}
```

---

### Profile

#### GET `/buyer/profile`
Get buyer profile.

---

#### PUT `/buyer/profile`
Update profile.

**Request:**
```json
{
  "ownerName": "Ramesh Kumar",
  "email": "ramesh@email.com"
}
```

---

### Addresses

#### GET `/buyer/addresses`
List saved addresses.

---

#### POST `/buyer/addresses`
Add address.

---

#### PUT `/buyer/addresses/:id`
Update address.

---

#### DELETE `/buyer/addresses/:id`
Delete address.

---

### Categories & Products

#### GET `/buyer/categories`
List categories with subcategories.

---

#### GET `/buyer/products`
List products with filters.

**Query:** `?category=cat_123&subcategory=subcat_456&search=parle&minPrice=10&maxPrice=100&inStock=true&sortBy=price&sortOrder=asc&page=1&limit=20`

---

#### GET `/buyer/products/:id`
Get product details.

---

#### GET `/buyer/products/featured`
Get featured products.

---

### Search

#### GET `/buyer/search/suggestions`
Get search suggestions.

**Query:** `?q=par`

**Response:**
```json
{
  "terms": ["parle g", "parle hide seek"],
  "products": [...],
  "categories": [...]
}
```

---

#### GET `/buyer/search/popular`
Get popular search terms.

---

### Cart

#### GET `/buyer/cart`
Get current cart.

---

#### POST `/buyer/cart/add`
Add item to cart.

**Request:**
```json
{
  "productId": "prod_123",
  "quantity": 24
}
```

---

#### PUT `/buyer/cart/items/:itemId`
Update item quantity.

**Request:**
```json
{
  "quantity": 36
}
```

---

#### DELETE `/buyer/cart/items/:itemId`
Remove item from cart.

---

#### DELETE `/buyer/cart`
Clear cart.

---

#### POST `/buyer/cart/check-serviceability`
Check delivery serviceability.

**Request:**
```json
{
  "pincode": "560001"
}
```

**Response:**
```json
{
  "serviceable": true,
  "estimatedDays": "2-3",
  "deliveryCharge": 0
}
```

---

### Orders

#### POST `/buyer/orders`
Create order.

**Request:**
```json
{
  "addressId": "addr_123",
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
    "total": 5250
  },
  "payment": {
    "method": "upi",
    "redirectUrl": "https://..."
  }
}
```

---

#### GET `/buyer/orders`
List buyer's orders.

**Query:** `?status=active&page=1&limit=10`

---

#### GET `/buyer/orders/:id`
Get order details.

---

#### POST `/buyer/orders/:id/cancel`
Cancel order (if eligible).

**Request:**
```json
{
  "reason": "Changed mind"
}
```

---

#### GET `/buyer/orders/:id/invoice`
Download invoice.

---

#### POST `/buyer/orders/:id/reorder`
Reorder previous order items.

---

### Notifications

#### GET `/buyer/notifications`
List notifications.

**Query:** `?page=1&limit=20&unreadOnly=true`

---

#### PUT `/buyer/notifications/:id/read`
Mark notification as read.

---

#### PUT `/buyer/notifications/read-all`
Mark all as read.

---

#### GET `/buyer/notifications/unread-count`
Get unread count.

---

### Credit

#### GET `/buyer/credit`
Get credit summary.

**Response:**
```json
{
  "limit": 25000,
  "available": 5000,
  "outstanding": 20000,
  "dueDate": "2024-02-15"
}
```

---

#### GET `/buyer/credit/ledger`
Get credit ledger.

---

## Common Response Formats

### Success Response
```json
{
  "success": true,
  "data": { ... },
  "message": "Operation successful"
}
```

### Error Response
```json
{
  "success": false,
  "error": "Error message",
  "code": "ERROR_CODE",
  "details": { ... }
}
```

### Paginated Response
```json
{
  "success": true,
  "data": {
    "items": [...],
    "pagination": {
      "total": 150,
      "page": 1,
      "limit": 25,
      "pages": 6
    }
  }
}
```

---

## Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| UNAUTHORIZED | 401 | Not authenticated |
| FORBIDDEN | 403 | No permission |
| NOT_FOUND | 404 | Resource not found |
| VALIDATION_ERROR | 400 | Invalid input |
| DUPLICATE | 409 | Already exists |
| SERVER_ERROR | 500 | Internal error |

---

## Rate Limiting

| Endpoint Type | Limit |
|--------------|-------|
| OTP Send | 5 per hour per phone |
| Authentication | 10 per minute per IP |
| General API | 100 per minute per user |
| Search | 30 per minute per user |

---

## Webhooks (Future)

### Payment Gateway Callback
`POST /webhooks/payment`

### SMS Delivery Status
`POST /webhooks/sms`
