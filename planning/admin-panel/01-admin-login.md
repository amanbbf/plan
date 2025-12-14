# Admin Panel - Login Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Admin Login |
| URL | `/admin/login` |
| Access | Public (unauthenticated) |
| Priority | P0 - Critical |
| Responsive | Desktop only (min 1024px) |

---

## Purpose
Allow admin users to securely login to the admin panel with email and password.

---

## UI Components

### 1. Header Section
- Company logo (centered)
- Page title: "Admin Portal"

### 2. Login Form
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Email | Email input | Valid email format | Yes |
| Password | Password input | Min 6 characters | Yes |
| Remember Me | Checkbox | - | No |

### 3. Action Buttons
- **Login** - Primary button, submits form
- **Forgot Password** - Text link, navigates to reset page

### 4. Error Messages
- Invalid credentials error
- Account locked error
- Network error

---

## User Flow

```
1. Admin opens /admin/login
2. Enters email and password
3. Clicks "Login" button
4. System validates credentials
   ├── Success → Redirect to Dashboard
   └── Failure → Show error message
5. Optional: Check "Remember Me" for persistent session
```

---

## API Calls

### POST `/api/admin/auth/login`

**Request:**
```json
{
  "email": "admin@example.com",
  "password": "password123",
  "rememberMe": true
}
```

**Success Response (200):**
```json
{
  "success": true,
  "token": "jwt_token_here",
  "user": {
    "id": "admin_id",
    "name": "Admin Name",
    "email": "admin@example.com",
    "role": "super_admin"
  }
}
```

**Error Response (401):**
```json
{
  "success": false,
  "error": "Invalid email or password"
}
```

---

## State Management

### Local State
- `email` - string
- `password` - string
- `rememberMe` - boolean
- `isLoading` - boolean
- `error` - string | null

### Global State (on success)
- Store JWT token in localStorage/cookie
- Store user info in global auth state
- Redirect to dashboard

---

## Validation Rules

| Field | Rules |
|-------|-------|
| Email | Required, valid email format |
| Password | Required, min 6 characters |

---

## Error Handling

| Error | Message |
|-------|---------|
| Invalid credentials | "Invalid email or password" |
| Account locked | "Account is locked. Contact super admin." |
| Network error | "Unable to connect. Please try again." |
| Too many attempts | "Too many login attempts. Try after 15 minutes." |

---

## Security Considerations

1. **Rate Limiting** - Max 5 failed attempts, then 15-min lockout
2. **HTTPS** - All requests over HTTPS
3. **Password Hashing** - Passwords never sent in plain text (use bcrypt on server)
4. **JWT Expiry** - Token expires in 24 hours (7 days if Remember Me)
5. **Secure Cookies** - HttpOnly, Secure, SameSite flags

---

## Design Specifications

### Layout
- Centered card on page
- Max width: 400px
- Padding: 40px
- Border radius: 8px
- Box shadow for depth

### Colors
- Background: Light gray (#f5f5f5)
- Card background: White
- Primary button: Brand color (e.g., #1976D2)
- Error text: Red (#d32f2f)

### Typography
- Title: 24px, bold
- Input labels: 14px, medium
- Button text: 16px, bold
- Error text: 14px

---

## Accessibility

- [ ] Form inputs have labels
- [ ] Error messages announced to screen readers
- [ ] Keyboard navigation works
- [ ] Focus states visible
- [ ] Color contrast meets WCAG AA

---

## Checklist

### Development
- [ ] Create login page UI
- [ ] Implement form validation
- [ ] Connect to login API
- [ ] Handle success redirect
- [ ] Handle error states
- [ ] Implement "Remember Me"
- [ ] Add loading state
- [ ] Add rate limiting on backend

### Testing
- [ ] Valid login works
- [ ] Invalid credentials show error
- [ ] Empty fields show validation
- [ ] Remember me persists session
- [ ] Redirect after login works
- [ ] Locked account handled
- [ ] Network error handled

---

## Dependencies
- Auth API endpoint
- JWT token handling utility
- Global auth state management
- Router for navigation
