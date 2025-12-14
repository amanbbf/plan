# Buyer App - Registration Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Buyer Registration |
| Route | `/register` or `RegisterScreen` |
| Access | Unauthenticated |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Allow new business customers to register with their business details for admin approval.

---

## UI Components

### Multi-Step Form

**Step 1: Phone Verification**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Phone Number | Number | 10 digits | Yes |
| OTP | 6 digits | After OTP sent | Yes |

**Step 2: Business Information**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Business Name | Text | 3-100 chars | Yes |
| Business Type | Dropdown | Select one | Yes |
| Owner Name | Text | 3-50 chars | Yes |
| Email | Email | Valid format | No |

**Step 3: GST & Documents**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| GST Number | Text | 15 chars, valid format | No |
| PAN Number | Text | 10 chars, valid format | No |
| Shop License | File | Image/PDF | No |

**Step 4: Address**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Address Line 1 | Text | 5-100 chars | Yes |
| Address Line 2 | Text | Max 100 chars | No |
| City | Text | 2-50 chars | Yes |
| State | Dropdown | Indian states | Yes |
| Pin Code | Number | 6 digits | Yes |
| Landmark | Text | Max 100 chars | No |

**Step 5: Password (Optional - if using password auth)**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Password | Password | Min 6 chars | Yes |
| Confirm Password | Password | Match password | Yes |

---

## Business Type Options
- Kirana / Grocery Store
- General Store
- Supermarket
- Hotel / Restaurant
- Canteen / Mess
- Bakery
- Sweet Shop
- Other Retail

---

## User Flow

```
1. Enter phone number
2. Receive & verify OTP
3. Fill business details
4. Enter GST (optional but validated)
5. Fill address
6. Submit registration
7. Show "Pending Approval" screen
8. Wait for admin approval
9. Receive notification on approval
10. Login with phone
```

---

## API Calls

### POST `/api/auth/send-otp`
Request OTP for registration.

**Request:**
```json
{
  "phone": "9876543210",
  "purpose": "register"
}
```

### POST `/api/auth/verify-otp`
Verify OTP.

**Request:**
```json
{
  "phone": "9876543210",
  "otp": "123456",
  "purpose": "register"
}
```

### POST `/api/auth/register`
Submit registration.

**Request:**
```json
{
  "phone": "9876543210",
  "business": {
    "name": "Krishna Stores",
    "type": "kirana",
    "ownerName": "Ramesh Kumar",
    "email": "ramesh@email.com",
    "gstNumber": "29AABCU9603R1ZM",
    "panNumber": "AABCU9603R"
  },
  "address": {
    "line1": "123 Main Road",
    "line2": "Near Bus Stand",
    "city": "Bangalore",
    "state": "Karnataka",
    "pincode": "560001",
    "landmark": "Opposite SBI Bank"
  }
}
```

**Success Response (201):**
```json
{
  "success": true,
  "message": "Registration successful. Pending approval.",
  "buyerId": "buyer_123"
}
```

**Error Response (409):**
```json
{
  "success": false,
  "error": "Phone number already registered"
}
```

---

## State Management

### Local State
- `step` - Current form step (1-4)
- `formData` - All form fields
- `errors` - Field-level errors
- `isLoading` - Loading state
- `isPhoneVerified` - OTP verified flag

### Form Data Structure
```javascript
{
  phone: "",
  otp: "",
  businessName: "",
  businessType: "",
  ownerName: "",
  email: "",
  gstNumber: "",
  panNumber: "",
  addressLine1: "",
  addressLine2: "",
  city: "",
  state: "",
  pincode: "",
  landmark: ""
}
```

---

## Validation Rules

### Phone
- Exactly 10 digits
- Numeric only
- Not already registered

### GST Number (if provided)
- Format: `\d{2}[A-Z]{5}\d{4}[A-Z]{1}[A-Z\d]{1}[Z]{1}[A-Z\d]{1}`
- Example: 29AABCU9603R1ZM
- First 2 digits = state code

### PAN Number (if provided)
- Format: `[A-Z]{5}[0-9]{4}[A-Z]{1}`
- Example: AABCU9603R

### Pin Code
- Exactly 6 digits
- Numeric only

### Email (if provided)
- Valid email format

---

## GST Validation

### State Code Matching
- First 2 digits of GST = State code
- Must match selected state
- Show warning if mismatch

### GST Format Validation
```
29 AABCU 9603 R 1 Z M
│  │     │    │ │ │ │
│  │     │    │ │ │ └── Check digit
│  │     │    │ │ └──── Default "Z"
│  │     │    │ └────── Entity type
│  │     │    └──────── 13th char
│  │     └───────────── Sequential
│  └─────────────────── PAN number
└────────────────────── State code
```

---

## Pin Code Serviceability Check

When pin code entered:
1. Check if serviceable
2. Show result:
   - ✅ "We deliver to this pin code"
   - ❌ "Sorry, we don't deliver to this area yet"
3. Allow registration even if not serviceable (for future)

---

## Progress Indicator

Visual stepper showing:
```
[1] Phone ─── [2] Business ─── [3] Documents ─── [4] Address
  ✓            ●                 ○                  ○
```

---

## Error Handling

| Error | Message |
|-------|---------|
| Phone exists | "This number is already registered. Please login." |
| Invalid GST | "Invalid GST format. Please check and try again." |
| GST state mismatch | "GST state code doesn't match selected state." |
| Invalid PIN | "Please enter a valid 6-digit pin code." |
| Network error | "Unable to connect. Please try again." |
| Server error | "Something went wrong. Please try later." |

---

## Post-Registration

### Success Screen
- Checkmark animation
- "Registration Successful!"
- "Your account is under review"
- "We'll notify you once approved (24-48 hours)"
- "Go to Login" button

### Notifications
- SMS: "Thank you for registering. Your account is under review."
- Push: Enabled for approval notification

---

## Design Specifications

### Layout
- Step indicator at top
- Form content scrollable
- Fixed bottom button
- Keyboard aware

### Progress
- Step numbers with labels
- Current step highlighted
- Completed steps with checkmark

### Form Fields
- Labels above inputs
- Error messages below inputs
- Optional label for non-required fields

---

## Checklist

### Development
- [ ] Create multi-step form
- [ ] Implement step navigation
- [ ] Add phone verification
- [ ] Create business info form
- [ ] Build GST/PAN validation
- [ ] Create address form
- [ ] Add state dropdown
- [ ] Implement pin code check
- [ ] Add form validation
- [ ] Create success screen
- [ ] Handle all error states
- [ ] Add loading states

### Testing
- [ ] Phone verification works
- [ ] Step navigation works
- [ ] All validations work
- [ ] GST validation correct
- [ ] Duplicate phone detected
- [ ] Registration submits
- [ ] Success screen shows
- [ ] Errors display properly

---

## Dependencies
- OTP service
- Form validation library
- State dropdown data
- Pin code serviceability API
- File upload (for documents)
- Navigation library
