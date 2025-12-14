# Admin Panel - Pin Code Serviceability Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Pin Code Serviceability |
| URL | `/admin/pincodes` |
| Access | Super Admin, Operations |
| Priority | P0 - Critical |
| Responsive | Desktop only |

---

## Purpose
Manage which pin codes are serviceable for delivery, set delivery charges, and estimated delivery times per zone.

---

## UI Components

### 1. Page Header
- Title: "Delivery Serviceability"
- "Add Pin Code" button
- "Bulk Upload" button
- Search bar (pin code)

### 2. Quick Stats
| Stat | Value |
|------|-------|
| Total Pin Codes | Count |
| Active | Serviceable count |
| Inactive | Non-serviceable |
| Zones | Zone count |

### 3. Zones Section (Optional)
Group pin codes into zones for easier management.

| Zone Name | Pin Codes | Delivery Days | Charge | Actions |
|-----------|-----------|---------------|--------|---------|
| Local (Same city) | 10 | 1-2 days | Free | Edit |
| District | 25 | 2-4 days | ₹30 | Edit |
| State | 50 | 4-7 days | ₹50 | Edit |

### 4. Pin Codes Table
| Column | Description | Sortable |
|--------|-------------|----------|
| Pin Code | 6-digit code | Yes |
| Area | Area name (if available) | Yes |
| Zone | Assigned zone | Yes |
| Delivery Days | Estimated days | Yes |
| Delivery Charge | ₹ amount | Yes |
| Status | Active/Inactive | Yes |
| Actions | Edit, Delete | No |

---

## Add/Edit Pin Code Modal

### Form Fields
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Pin Code | Text | 6 digits, numeric | Yes |
| Area Name | Text | Max 100 chars | No |
| City | Text | Max 50 chars | No |
| State | Dropdown | Indian states | Yes |
| Zone | Dropdown | Existing zones | No |
| Delivery Days (Min) | Number | 1-30 | Yes |
| Delivery Days (Max) | Number | >= Min | Yes |
| Delivery Charge | Number | >= 0 | Yes |
| Free Delivery Above | Number | Order amount | No |
| Status | Toggle | - | Yes |

---

## Zone Management

### Create Zone
| Field | Type | Required |
|-------|------|----------|
| Zone Name | Text | Yes |
| Description | Text | No |
| Delivery Days (Min) | Number | Yes |
| Delivery Days (Max) | Number | Yes |
| Default Charge | Number | Yes |

### Assign Pin Codes to Zone
- Multi-select pin codes
- Assign to zone
- Inherits zone's delivery settings

---

## Bulk Upload

### CSV Template
```csv
pincode,area,city,state,deliveryDaysMin,deliveryDaysMax,deliveryCharge,status
560001,MG Road,Bangalore,Karnataka,1,2,0,active
560002,Indiranagar,Bangalore,Karnataka,1,2,0,active
560003,Koramangala,Bangalore,Karnataka,2,3,30,active
```

### Upload Flow
1. Download template
2. Fill pin code data
3. Upload CSV
4. Validate & preview
5. Confirm import

---

## API Calls

### GET `/api/admin/pincodes`
List all pin codes with pagination and filters.

### POST `/api/admin/pincodes`
Add new pin code.

### PUT `/api/admin/pincodes/:id`
Update pin code.

### DELETE `/api/admin/pincodes/:id`
Remove pin code.

### POST `/api/admin/pincodes/bulk-upload`
Bulk upload pin codes.

### GET `/api/admin/pincodes/check/:pincode`
Check if pin code is serviceable (public API too).

### Zones

### GET `/api/admin/zones`
List all zones.

### POST `/api/admin/zones`
Create zone.

### PUT `/api/admin/zones/:id`
Update zone.

### PUT `/api/admin/zones/:id/assign`
Assign pin codes to zone.

---

## Pin Code Data Structure

```json
{
  "_id": "pin_123",
  "pincode": "560001",
  "area": "MG Road",
  "city": "Bangalore",
  "state": "Karnataka",
  "zone": "zone_local",
  "delivery": {
    "daysMin": 1,
    "daysMax": 2,
    "charge": 0,
    "freeAbove": null
  },
  "status": "active",
  "createdAt": "...",
  "updatedAt": "..."
}
```

---

## Serviceability Check Logic

When buyer enters pin code:
```
1. Check if pin code exists in database
2. Check if status = active
3. Return:
   - serviceable: true/false
   - estimatedDelivery: "1-2 days"
   - deliveryCharge: 0 or amount
   - freeDeliveryAbove: amount (if applicable)
```

---

## Business Rules

### Delivery Charge Calculation
1. Check cart total
2. If `freeAbove` set and cart >= freeAbove → charge = 0
3. Else use pin code delivery charge
4. Zone default used if pin code doesn't have specific charge

### Non-Serviceable Response
- Clear message: "We don't deliver to this pin code yet"
- Suggest contacting support
- Option to request service

---

## Validation Rules

| Field | Rule |
|-------|------|
| Pin Code | Exactly 6 digits |
| Pin Code | Must be unique |
| Delivery Days Min | >= 1 |
| Delivery Days Max | >= Min |
| Delivery Charge | >= 0 |

---

## Design Specifications

### Table
- Pin codes grouped by zone (optional toggle)
- Active status: Green indicator
- Inactive: Gray indicator

### Search
- Real-time search as user types
- Filter by status

---

## Checklist

### Development
- [ ] Create pin code list page
- [ ] Add/edit pin code modal
- [ ] Implement zone management
- [ ] Build bulk upload feature
- [ ] Create serviceability API
- [ ] Add search and filters
- [ ] Connect to checkout flow
- [ ] Add validation

### Testing
- [ ] Add pin code works
- [ ] Edit pin code works
- [ ] Delete pin code works
- [ ] Bulk upload works
- [ ] Zone assignment works
- [ ] Serviceability check works
- [ ] Delivery charge calculates correctly
- [ ] Free delivery threshold works

---

## Dependencies
- CSV parser
- Indian states data
- Checkout integration
- Delivery calculation service
