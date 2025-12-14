# Admin Panel - Settings Pages

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Settings |
| URL | `/admin/settings/*` |
| Access | Super Admin only |
| Priority | P1 - Important |
| Responsive | Desktop only |

---

## Purpose
Configure system-wide settings, company information, tax rules, admin users, and app configurations.

---

## Settings Sub-Pages

### 1. Company Profile (`/admin/settings/company`)
### 2. Tax Settings (`/admin/settings/tax`)
### 3. Admin Users (`/admin/settings/users`)
### 4. Courier Services (`/admin/settings/couriers`)
### 5. App Settings (`/admin/settings/app`)
### 6. Notifications (`/admin/settings/notifications`)

---

## 1. Company Profile

### Form Fields
| Field | Type | Required |
|-------|------|----------|
| Company Name | Text | Yes |
| Legal Name | Text | Yes |
| GSTIN | Text | Yes |
| PAN | Text | Yes |
| Address Line 1 | Text | Yes |
| Address Line 2 | Text | No |
| City | Text | Yes |
| State | Dropdown | Yes |
| Pin Code | Text | Yes |
| Phone | Text | Yes |
| Email | Email | Yes |
| Website | URL | No |
| Logo | File upload | No |

### Bank Details (for invoices)
| Field | Type | Required |
|-------|------|----------|
| Bank Name | Text | Yes |
| Account Number | Text | Yes |
| IFSC Code | Text | Yes |
| Branch | Text | No |
| UPI ID | Text | No |

---

## 2. Tax Settings

### GST Configuration
| Setting | Value |
|---------|-------|
| Default GST Rate | Dropdown (5/12/18/28%) |
| State Code | 2-digit code |
| Enable HSN Validation | Toggle |

### GST Rates Table
| Rate | Description | Default |
|------|-------------|---------|
| 0% | Exempt items | - |
| 5% | Essential foods | ✓ |
| 12% | Processed foods | - |
| 18% | Standard rate | - |
| 28% | Luxury items | - |

### Invoice Settings
| Setting | Type | Default |
|---------|------|---------|
| Invoice Prefix | Text | INV |
| Invoice Starting Number | Number | 1 |
| Financial Year Start | Month | April |
| Auto-generate on Confirm | Toggle | Yes |
| Show Bank Details | Toggle | Yes |
| Terms & Conditions | Textarea | - |

---

## 3. Admin Users

### Users List
| Column | Description |
|--------|-------------|
| Name | Admin name |
| Email | Login email |
| Role | Super Admin / Operations / Finance |
| Status | Active / Inactive |
| Last Login | Date/time |
| Actions | Edit, Delete, Reset Password |

### Add/Edit Admin User
| Field | Type | Required |
|-------|------|----------|
| Full Name | Text | Yes |
| Email | Email | Yes |
| Phone | Text | Yes |
| Role | Dropdown | Yes |
| Password | Password | Yes (new) |
| Status | Toggle | Yes |

### Roles & Permissions

**Super Admin**
- Full system access
- All CRUD operations
- Settings management
- User management

**Operations**
- Products: Full access
- Orders: Full access
- Inventory: Full access
- Buyers: View + Approve
- Shipping: Full access
- Settings: No access

**Finance**
- Orders: View only
- Payments: Full access
- Invoices: Full access
- Reports: Full access
- Buyers: View + Credit management
- Settings: No access

---

## 4. Courier Services

### Couriers Table
| Column | Description |
|--------|-------------|
| Name | Courier name |
| Code | Short code |
| Tracking URL | URL template |
| Status | Active/Inactive |
| Actions | Edit, Delete |

### Add/Edit Courier
| Field | Type | Required |
|-------|------|----------|
| Courier Name | Text | Yes |
| Short Code | Text | Yes |
| Tracking URL | Text | Yes |
| Contact Phone | Text | No |
| Contact Email | Email | No |
| Notes | Textarea | No |
| Status | Toggle | Yes |

### Tracking URL Examples
| Courier | URL Template |
|---------|-------------|
| India Post | `https://www.indiapost.gov.in/VAS/Pages/trackconsignment.aspx?id={{awb}}` |
| DTDC | `https://www.dtdc.in/tracking.asp?strCnno={{awb}}` |
| Delhivery | `https://www.delhivery.com/track/package/{{awb}}` |

---

## 5. App Settings

### General Settings
| Setting | Type | Description |
|---------|------|-------------|
| App Name | Text | Display name |
| Support Phone | Text | Help contact |
| Support Email | Email | Help email |
| WhatsApp Number | Text | WhatsApp support |

### Ordering Settings
| Setting | Type | Default |
|---------|------|---------|
| Minimum Order Value | Number | ₹500 |
| Maximum Order Value | Number | ₹50,000 |
| Default MOQ | Number | 1 |
| Allow Backorder | Toggle | No |
| Show Stock Levels | Toggle | Yes |

### Credit Settings
| Setting | Type | Default |
|---------|------|---------|
| Default Credit Limit | Number | ₹0 |
| Maximum Credit Limit | Number | ₹1,00,000 |
| Credit Period (Days) | Number | 30 |
| Block on Overdue | Toggle | Yes |

### Delivery Settings
| Setting | Type | Default |
|---------|------|---------|
| Default Delivery Charge | Number | ₹50 |
| Free Delivery Above | Number | ₹2000 |
| Max Delivery Days | Number | 7 |

---

## 6. Notification Settings

### Email Notifications
| Event | Admin | Buyer | Toggle |
|-------|-------|-------|--------|
| New Registration | ✅ | ❌ | On |
| Account Approved | ❌ | ✅ | On |
| Order Placed | ✅ | ✅ | On |
| Order Shipped | ❌ | ✅ | On |
| Order Delivered | ❌ | ✅ | On |
| Payment Received | ✅ | ✅ | On |

### Push Notifications
| Event | Admin App | Buyer App |
|-------|-----------|-----------|
| New Order | ✅ | - |
| Order Status | - | ✅ |
| Low Stock Alert | ✅ | - |
| Payment Reminder | - | ✅ |

### SMS Settings (Future)
| Setting | Type |
|---------|------|
| SMS Provider | Dropdown |
| API Key | Text |
| Sender ID | Text |

---

## API Calls

### GET `/api/admin/settings/:section`
Get settings for a section.

### PUT `/api/admin/settings/:section`
Update settings.

### Admin Users

### GET `/api/admin/users`
List admin users.

### POST `/api/admin/users`
Create admin user.

### PUT `/api/admin/users/:id`
Update admin user.

### DELETE `/api/admin/users/:id`
Delete admin user (soft delete).

### PUT `/api/admin/users/:id/password`
Reset password.

---

## Settings Data Structure

```json
{
  "_id": "settings",
  "company": {
    "name": "ABC Wholesale",
    "legalName": "ABC Trading Pvt Ltd",
    "gstin": "29AABCU9603R1ZM",
    "pan": "AABCU9603R",
    "address": {...},
    "phone": "...",
    "email": "...",
    "logo": "url",
    "bank": {...}
  },
  "tax": {
    "defaultRate": 5,
    "stateCode": "29",
    "invoicePrefix": "INV",
    "invoiceStartNumber": 1,
    "fyStart": "april"
  },
  "ordering": {
    "minOrderValue": 500,
    "maxOrderValue": 50000,
    "defaultMoq": 1,
    "allowBackorder": false
  },
  "credit": {
    "defaultLimit": 0,
    "maxLimit": 100000,
    "period": 30,
    "blockOnOverdue": true
  },
  "delivery": {
    "defaultCharge": 50,
    "freeAbove": 2000,
    "maxDays": 7
  },
  "notifications": {
    "email": {...},
    "push": {...}
  }
}
```

---

## Checklist

### Development
- [ ] Create settings sidebar navigation
- [ ] Build company profile form
- [ ] Create tax settings page
- [ ] Build admin users management
- [ ] Implement role-based permissions
- [ ] Create courier management
- [ ] Build app settings page
- [ ] Add notification settings
- [ ] Implement logo upload
- [ ] Add validation for all fields

### Testing
- [ ] Company profile saves correctly
- [ ] Tax settings apply to invoices
- [ ] Admin users CRUD works
- [ ] Role permissions enforced
- [ ] Courier configuration works
- [ ] App settings apply correctly
- [ ] Notification toggles work

---

## Dependencies
- File upload service (logo)
- Role-based access control
- Form validation
- Settings API
