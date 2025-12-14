# Buyer App - Profile Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Profile / My Account |
| Route | `/profile` or `ProfileScreen` |
| Access | Authenticated buyers |
| Priority | P1 - Important |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
View and manage account information, business details, addresses, credit info, and app settings.

---

## UI Components

### 1. Profile Header
```
┌─────────────────────────────────────┐
│ [Avatar]  Krishna Stores            │
│           Ramesh Kumar              │
│           +91 9876543210            │
│           [Edit Profile]            │
└─────────────────────────────────────┘
```

### 2. Credit Card (if credit enabled)
```
┌─────────────────────────────────────┐
│ Credit Available                    │
│ ₹5,000                              │
│ ───────────────────                 │
│ Limit: ₹10,000 | Due: ₹2,500        │
│                                     │
│ [View Ledger]                       │
└─────────────────────────────────────┘
```

### 3. Menu Options
```
Account
├── Business Details
├── Saved Addresses
├── GST Information
└── Staff Management (if owner)

Orders & Payments
├── My Orders
├── Payment History
└── Credit Ledger

Preferences
├── Notifications
├── Language
└── App Theme (future)

Support
├── Help & FAQ
├── Contact Support
└── About Us

[Logout]
```

---

## Profile Sections

### 1. Business Details
| Field | Editable |
|-------|----------|
| Business Name | No (contact admin) |
| Business Type | No |
| Owner Name | Yes |
| Email | Yes |
| Alternate Phone | Yes |

### 2. Saved Addresses
- List of addresses
- Add new address
- Edit address
- Delete address
- Set default

### 3. GST Information
| Field | Display |
|-------|---------|
| GST Number | 29AABCU9603R1ZM |
| PAN Number | AABCU9603R |
| State | Karnataka |

Note: "Contact admin to update"

### 4. Staff Management (Owner only)
- View staff accounts
- Add staff (future)
- Remove staff access

---

## Credit Ledger View

### Ledger Table
| Date | Description | Debit | Credit | Balance |
|------|-------------|-------|--------|---------|
| 15 Jan | Order #001 | ₹5,250 | - | ₹15,000 |
| 10 Jan | Payment | - | ₹10,000 | ₹20,250 |
| 05 Jan | Order #002 | ₹3,000 | - | ₹10,250 |

### Summary
- Credit Limit: ₹25,000
- Used: ₹20,000
- Available: ₹5,000
- Overdue: ₹0

---

## API Calls

### GET `/api/profile`
Get buyer profile.

### PUT `/api/profile`
Update profile.

### GET `/api/addresses`
Get saved addresses.

### GET `/api/credit/ledger`
Get credit ledger.

### POST `/api/auth/logout`
Logout user.

---

## State Management

### Local State
- `profile` - User profile data
- `isEditing` - Edit mode
- `isLoading` - Loading state

---

## Edit Profile Flow

1. Tap "Edit Profile"
2. Enable editable fields
3. Make changes
4. Tap "Save"
5. Validate inputs
6. Call update API
7. Show success message

### Editable Fields
- Owner name
- Email
- Alternate phone

### Non-Editable
- Phone (primary)
- Business name (contact admin)
- GST number (contact admin)

---

## Logout Flow

1. Tap "Logout"
2. Confirm: "Are you sure you want to logout?"
3. On confirm:
   - Clear auth token
   - Clear local data
   - Navigate to login

---

## Notification Settings

### Toggles
| Notification | Default |
|--------------|---------|
| Order updates | On |
| Promotional offers | On |
| Payment reminders | On |
| New products | Off |

---

## Help & Support

### Options
- FAQs (common questions)
- Call support (phone number)
- WhatsApp support (if available)
- Email support

---

## Design Specifications

### Layout
- Header with avatar
- Credit card prominent
- Menu list with sections
- Logout at bottom

### Profile Header
- Avatar: 80px circle
- Business name: Bold, 18px
- Owner name: Regular, 14px
- Phone: Gray, 14px

### Credit Card
- Gradient background
- Large available amount
- Progress bar for usage

### Menu Items
- Section headings
- Icon + Label + Chevron
- Grouped with spacing

---

## Checklist

### Development
- [ ] Create profile screen
- [ ] Display profile header
- [ ] Show credit card
- [ ] Build menu navigation
- [ ] Create business details view
- [ ] Implement edit profile
- [ ] Create addresses list
- [ ] Build credit ledger view
- [ ] Add notification settings
- [ ] Implement logout
- [ ] Help & support section

### Testing
- [ ] Profile loads correctly
- [ ] Edit profile works
- [ ] Addresses display
- [ ] Credit info accurate
- [ ] Navigation works
- [ ] Logout works
- [ ] Settings save

---

## Dependencies
- Profile API
- Addresses API
- Credit API
- Auth state management
- Settings storage
