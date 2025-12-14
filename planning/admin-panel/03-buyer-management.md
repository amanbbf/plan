# Admin Panel - Buyer Management Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Buyer Management |
| URL | `/admin/buyers` |
| Access | Super Admin, Operations |
| Priority | P0 - Critical |
| Responsive | Desktop only |

---

## Purpose
View, manage, and approve/reject buyer registrations. Manage buyer accounts, credit limits, and access.

---

## UI Components

### 1. Page Header
- Title: "Buyer Management"
- Search bar (search by name, phone, GST)
- Filter dropdown (Status: All, Pending, Approved, Blocked)
- Export button (CSV)

### 2. Tabs
- **All Buyers** - Complete list
- **Pending Approval** - Awaiting approval (with count badge)
- **Blocked** - Suspended accounts

### 3. Buyers Table
| Column | Description | Sortable |
|--------|-------------|----------|
| S.No | Row number | No |
| Business Name | Company/shop name | Yes |
| Owner Name | Contact person | Yes |
| Phone | Primary phone | No |
| GST Number | GSTIN (if provided) | No |
| City | Location | Yes |
| Status | Active/Pending/Blocked badge | Yes |
| Credit Limit | ₹ amount | Yes |
| Joined | Registration date | Yes |
| Actions | View, Edit, Block/Unblock | No |

### 4. Pagination
- 25 rows per page default
- Page navigation
- "Showing X to Y of Z buyers"

### 5. Quick Stats (above table)
- Total Buyers
- Active Buyers
- Pending Approvals
- Blocked Accounts

---

## Buyer Details Modal/Page

When clicking "View" on a buyer:

### Business Information
| Field | Value |
|-------|-------|
| Business Name | Shop/Company name |
| Business Type | Retailer/Hotel/etc |
| GST Number | GSTIN |
| PAN Number | (if available) |
| Address | Full address with pin code |

### Contact Information
| Field | Value |
|-------|-------|
| Owner Name | Full name |
| Phone | Primary number |
| Alt Phone | Secondary number |
| Email | (if available) |

### Account Information
| Field | Value |
|-------|-------|
| User ID | Unique ID |
| Status | Current status |
| Registered | Date & time |
| Approved By | Admin who approved |
| Last Order | Date of last order |
| Total Orders | Order count |
| Total Spent | Lifetime value |

### Credit Information
| Field | Value |
|-------|-------|
| Credit Limit | ₹ amount |
| Outstanding | Current due amount |
| Available Credit | Remaining credit |
| Last Payment | Date & amount |

### Staff Accounts (if any)
- List of staff users
- Add/remove staff option

### Actions
- Approve (if pending)
- Reject (if pending)
- Edit Details
- Update Credit Limit
- Block/Unblock
- Reset Password
- View Order History

---

## Approval Workflow

### Pending Buyer Row
- Highlighted in yellow
- "Approve" and "Reject" buttons visible

### Approval Modal
- Review all business details
- GST verification status
- Notes field for admin
- Approve / Reject buttons

### On Approval
- Status changes to Active
- Welcome notification sent to buyer
- Credit limit can be set

### On Rejection
- Status changes to Rejected
- Rejection reason required
- Buyer notified with reason

---

## API Calls

### GET `/api/admin/buyers`
**Query Params:**
- `page` - Page number
- `limit` - Items per page
- `status` - Filter by status
- `search` - Search term
- `sortBy` - Sort field
- `sortOrder` - asc/desc

**Response:**
```json
{
  "success": true,
  "data": {
    "buyers": [...],
    "pagination": {
      "total": 150,
      "page": 1,
      "limit": 25,
      "pages": 6
    }
  }
}
```

### GET `/api/admin/buyers/:id`
Returns single buyer details.

### PUT `/api/admin/buyers/:id/approve`
Approve pending buyer.

### PUT `/api/admin/buyers/:id/reject`
Reject pending buyer (with reason).

### PUT `/api/admin/buyers/:id`
Update buyer details.

### PUT `/api/admin/buyers/:id/credit-limit`
Update credit limit.

### PUT `/api/admin/buyers/:id/block`
Block buyer account.

### PUT `/api/admin/buyers/:id/unblock`
Unblock buyer account.

### GET `/api/admin/buyers/stats`
Returns buyer statistics.

---

## State Management

### Local State
- `buyers` - Array of buyer objects
- `selectedBuyer` - Currently viewed buyer
- `pagination` - Pagination state
- `filters` - Active filters
- `searchTerm` - Search input
- `isLoading` - Loading state
- `showModal` - Modal visibility

---

## Filter Options

### Status Filter
- All
- Pending Approval
- Active
- Blocked
- Rejected

### Sort Options
- Business Name (A-Z, Z-A)
- Joined Date (Newest, Oldest)
- Credit Limit (High-Low, Low-High)
- Total Orders (High-Low)

---

## Actions & Permissions

| Action | Super Admin | Operations | Finance |
|--------|-------------|------------|---------|
| View Buyers | ✅ | ✅ | ✅ |
| Approve/Reject | ✅ | ✅ | ❌ |
| Edit Details | ✅ | ✅ | ❌ |
| Update Credit | ✅ | ❌ | ✅ |
| Block/Unblock | ✅ | ✅ | ❌ |
| Export Data | ✅ | ✅ | ✅ |

---

## Validation Rules

### Credit Limit Update
- Minimum: ₹0
- Maximum: ₹10,00,000
- Must be a whole number

### Rejection Reason
- Required when rejecting
- Minimum 10 characters

---

## Notifications Triggered

| Action | Notification |
|--------|--------------|
| Buyer Approved | SMS/Push to buyer |
| Buyer Rejected | SMS/Push with reason |
| Credit Updated | Push to buyer |
| Account Blocked | SMS/Push to buyer |

---

## Design Specifications

### Table Design
- Striped rows for readability
- Hover effect on rows
- Status badges with colors:
  - Active: Green
  - Pending: Yellow/Orange
  - Blocked: Red
  - Rejected: Gray

### Modal Design
- Slide-in from right or center modal
- Sections with clear headings
- Sticky action buttons at bottom

---

## Checklist

### Development
- [ ] Create buyers list page
- [ ] Implement search functionality
- [ ] Add status filters
- [ ] Create pagination
- [ ] Build buyer details view
- [ ] Implement approval workflow
- [ ] Add rejection with reason
- [ ] Create credit limit update
- [ ] Implement block/unblock
- [ ] Add role-based permissions
- [ ] Create export functionality

### Testing
- [ ] List loads correctly
- [ ] Search works
- [ ] Filters work
- [ ] Pagination works
- [ ] Approval flow works
- [ ] Rejection flow works
- [ ] Credit update works
- [ ] Block/unblock works
- [ ] Permissions enforced
- [ ] Notifications sent

---

## Dependencies
- Buyer API endpoints
- Role-based access control
- Notification service
- CSV export utility
- Modal/Drawer component
