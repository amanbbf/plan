# Buyer App - Notifications Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Notifications |
| Route | `/notifications` or `NotificationsScreen` |
| Access | Authenticated buyers |
| Priority | P1 - Important |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Display all notifications about orders, payments, promotions, and account updates.

---

## UI Components

### 1. Header
- Back button
- Title: "Notifications"
- Mark all read (if unread exist)

### 2. Notification List
```
┌─────────────────────────────────────┐
│ 📦 Order Shipped                    │
│ Your order #ORD-2024-001 has been   │
│ shipped. Track: EE123456789IN       │
│                                     │
│ 2 hours ago                         │
└─────────────────────────────────────┘
┌─────────────────────────────────────┐
│ ✓ Order Confirmed                   │
│ Your order #ORD-2024-001 has been   │
│ confirmed and is being prepared.    │
│                                     │
│ Yesterday                           │
└─────────────────────────────────────┘
```

### 3. Empty State
- Bell icon
- "No notifications yet"
- "Your order updates will appear here"

---

## Notification Types

### Order Notifications
| Type | Icon | Title |
|------|------|-------|
| order_placed | 📝 | Order Placed |
| order_confirmed | ✓ | Order Confirmed |
| order_shipped | 📦 | Order Shipped |
| order_delivered | 🎉 | Order Delivered |
| order_cancelled | ❌ | Order Cancelled |

### Payment Notifications
| Type | Icon | Title |
|------|------|-------|
| payment_success | 💰 | Payment Successful |
| payment_failed | ⚠️ | Payment Failed |
| payment_reminder | ⏰ | Payment Reminder |
| credit_updated | 💳 | Credit Limit Updated |

### Account Notifications
| Type | Icon | Title |
|------|------|-------|
| account_approved | 🎉 | Account Approved |
| profile_updated | 👤 | Profile Updated |

### Promotional (Optional)
| Type | Icon | Title |
|------|------|-------|
| new_offer | 🏷️ | New Offer |
| new_products | 🆕 | New Products |

---

## Notification Data Structure

```json
{
  "id": "notif_123",
  "type": "order_shipped",
  "title": "Order Shipped",
  "body": "Your order #ORD-2024-001 has been shipped.",
  "data": {
    "orderId": "order_123",
    "trackingNumber": "EE123456789IN"
  },
  "read": false,
  "createdAt": "2024-01-16T16:30:00Z"
}
```

---

## API Calls

### GET `/api/notifications`
Get notifications list.

**Query:** `page`, `limit`, `unreadOnly`

**Response:**
```json
{
  "success": true,
  "data": {
    "notifications": [...],
    "unreadCount": 5,
    "pagination": {...}
  }
}
```

### PUT `/api/notifications/:id/read`
Mark notification as read.

### PUT `/api/notifications/read-all`
Mark all as read.

---

## State Management

### Local State
- `notifications` - Array
- `unreadCount` - Badge count
- `isLoading` - Loading state
- `pagination` - Pagination state

### Global State
- Unread count for badge

---

## Notification Actions

When tapped, navigate based on type:

| Type | Navigation |
|------|------------|
| order_* | Order Details |
| payment_* | Order Details or Profile |
| account_* | Profile |
| promotional | Product or Category |

---

## Push Notifications

### Setup
- Request permission on first launch
- Register device token
- Send token to backend

### Received Notification
- Show system notification
- Update badge count
- Tap → Open app to relevant screen

---

## Mark as Read

### Auto-mark
- Mark read when notification opened/tapped

### Manual
- "Mark all as read" button
- Swipe to mark read (optional)

---

## Design Specifications

### Layout
- Full screen list
- Pull to refresh
- Infinite scroll

### Notification Card
- Icon left (type-specific)
- Title bold
- Body 2 lines max
- Timestamp right-aligned or below
- Unread: Blue left border or dot

### Unread Indicator
- Blue dot or border
- Different background color

### Timestamp
- Relative: "2 hours ago", "Yesterday"
- After 7 days: Show date

---

## Checklist

### Development
- [ ] Create notifications screen
- [ ] Fetch notifications
- [ ] Display notification list
- [ ] Implement pagination
- [ ] Mark as read on tap
- [ ] Mark all as read
- [ ] Navigate on tap
- [ ] Empty state
- [ ] Pull to refresh
- [ ] Update badge count
- [ ] Push notification setup

### Testing
- [ ] Notifications load
- [ ] Pagination works
- [ ] Mark read works
- [ ] Navigation works
- [ ] Badge updates
- [ ] Push notifications work

---

## Dependencies
- Notifications API
- Push notification library (Expo Push)
- Navigation
- Badge/count state
