# Buyer App - Login Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Buyer Login |
| Route | `/login` or `LoginScreen` |
| Access | Unauthenticated |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Allow registered buyers to login to the app using phone number and OTP verification.

---

## UI Components

### 1. Header Section
- App logo (centered)
- Tagline: "B2B Grocery Wholesale"

### 2. Login Form

**Step 1: Phone Number**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Phone Number | Number input | 10 digits, Indian format | Yes |
| Country Code | Display only | +91 | - |

**Step 2: OTP Verification**
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| OTP | 6-digit input | Numeric, 6 digits | Yes |

### 3. Action Buttons
- **Send OTP** - Requests OTP (Step 1)
- **Verify & Login** - Verifies OTP (Step 2)
- **Resend OTP** - Appears after 30 sec countdown

### 4. Additional Links
- "New to [App Name]? Register" → Registration page
- "Need Help?" → Support contact

### 5. Status Messages
- OTP sent successfully
- Invalid phone number
- Invalid OTP
- Account pending approval
- Account blocked

---

## User Flow

```
1. User opens app
2. Enters 10-digit phone number
3. Taps "Send OTP"
4. Receives OTP via SMS
5. Enters 6-digit OTP
6. Taps "Verify & Login"
   ├── Success → Check account status
   │   ├── Active → Navigate to Home
   │   ├── Pending → Show "Approval Pending" screen
   │   └── Blocked → Show "Account Blocked" message
   └── Failure → Show error, retry
```

---

## API Calls

### POST `/api/auth/send-otp`
Request OTP for login.

**Request:**
```json
{
  "phone": "9876543210",
  "purpose": "login"
}
```

**Success Response (200):**
```json
{
  "success": true,
  "message": "OTP sent successfully",
  "expiresIn": 300
}
```

**Error Response (404):**
```json
{
  "success": false,
  "error": "Phone number not registered"
}
```

### POST `/api/auth/verify-otp`
Verify OTP and login.

**Request:**
```json
{
  "phone": "9876543210",
  "otp": "123456",
  "purpose": "login"
}
```

**Success Response (200):**
```json
{
  "success": true,
  "token": "jwt_token",
  "user": {
    "id": "buyer_123",
    "businessName": "Krishna Stores",
    "phone": "9876543210",
    "status": "active",
    "role": "owner"
  }
}
```

**Account Pending (403):**
```json
{
  "success": false,
  "error": "Account pending approval",
  "status": "pending"
}
```

---

## State Management

### Local State
- `phone` - Phone number input
- `otp` - OTP input
- `step` - Current step (phone/otp)
- `isLoading` - Loading state
- `error` - Error message
- `countdown` - Resend countdown timer

### Global State (on success)
- Store JWT token securely
- Store user info
- Navigate to appropriate screen

---

## Validation Rules

| Field | Rules |
|-------|-------|
| Phone | Required, exactly 10 digits, numeric only |
| OTP | Required, exactly 6 digits, numeric only |

---

## Error Handling

| Error | Message | Action |
|-------|---------|--------|
| Phone not registered | "This number is not registered. Please register first." | Show Register link |
| Invalid OTP | "Invalid OTP. Please try again." | Clear OTP, refocus |
| OTP expired | "OTP has expired. Please request a new one." | Show Resend button |
| Account pending | "Your account is pending approval." | Show Pending screen |
| Account blocked | "Your account has been suspended. Contact support." | Show Support contact |
| Network error | "Unable to connect. Check your internet." | Retry button |

---

## OTP Behavior

### OTP Timer
- 30-second countdown before resend enabled
- OTP valid for 5 minutes
- Maximum 5 OTP requests per hour

### Resend Logic
1. Show countdown: "Resend OTP in 30s"
2. After countdown: Show "Resend OTP" button
3. On resend: Reset countdown, send new OTP

---

## Account Status Handling

| Status | Behavior |
|--------|----------|
| active | Login successful, navigate to Home |
| pending | Show "Pending Approval" screen |
| blocked | Show error, contact support |

### Pending Approval Screen
- Message: "Your account is under review"
- "We'll notify you once approved"
- "Contact support if taking too long"
- Estimated time: 24-48 hours

---

## Design Specifications

### Layout (Mobile)
- Logo at top (20% height)
- Form centered
- Keyboard-aware scroll
- Safe area padding

### Colors
- Primary: Brand color
- Background: White
- Input border: Gray
- Error: Red
- Success: Green

### Typography
- Logo: Image
- Title: 24px, bold
- Input: 16px
- Button: 16px, bold
- Links: 14px, brand color

### OTP Input
- 6 separate boxes
- Auto-focus next on input
- Paste support

---

## Accessibility

- [ ] Input labels for screen readers
- [ ] Error announcements
- [ ] Keyboard navigation
- [ ] Sufficient color contrast
- [ ] Touch targets 44px minimum

---

## Checklist

### Development
- [ ] Create login screen UI
- [ ] Implement phone input with validation
- [ ] Add Send OTP functionality
- [ ] Create OTP input component
- [ ] Implement OTP verification
- [ ] Add resend countdown timer
- [ ] Handle all error states
- [ ] Handle account status
- [ ] Store auth token securely
- [ ] Navigate to appropriate screen
- [ ] Add loading states

### Testing
- [ ] Valid login flow works
- [ ] Invalid phone shows error
- [ ] Invalid OTP shows error
- [ ] Resend timer works
- [ ] Pending account handled
- [ ] Blocked account handled
- [ ] Network error handled
- [ ] Token stored correctly
- [ ] Navigation works

---

## Dependencies
- OTP service (SMS gateway)
- Secure token storage (AsyncStorage/SecureStore)
- Navigation library
- Form validation
- Timer utility
