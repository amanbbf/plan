# Testing Checklist

## Authentication Tests

### Admin Authentication
- [ ] Admin can login with valid credentials
- [ ] Invalid credentials show error
- [ ] Token is returned on success
- [ ] Token includes role information
- [ ] Expired token is rejected
- [ ] Password reset works
- [ ] Session timeout works

### Buyer Authentication
- [ ] OTP is sent to valid phone
- [ ] OTP not sent to invalid phone
- [ ] OTP verification works
- [ ] Wrong OTP shows error
- [ ] Expired OTP shows error
- [ ] Registration creates pending account
- [ ] Pending account cannot login
- [ ] Approved account can login
- [ ] Blocked account shows error

---

## Admin Panel Tests

### Dashboard
- [ ] Stats display correctly
- [ ] Sales chart loads
- [ ] Order status chart loads
- [ ] Recent orders table loads
- [ ] Navigation to order details works
- [ ] Refresh updates data

### Buyer Management
- [ ] Buyer list loads
- [ ] Pagination works
- [ ] Search works
- [ ] Status filter works
- [ ] Buyer details modal opens
- [ ] Approve buyer works
- [ ] Reject buyer requires reason
- [ ] Rejection reason saved
- [ ] Credit limit update works
- [ ] Block/unblock works
- [ ] Notification sent on approval

### Category Management
- [ ] Categories list loads
- [ ] Add category works
- [ ] Edit category works
- [ ] Image upload works
- [ ] Delete empty category works
- [ ] Delete with products blocked
- [ ] Subcategories display
- [ ] Add subcategory works
- [ ] Edit subcategory works
- [ ] Sort order updates

### Product Management
- [ ] Products list loads
- [ ] Search works
- [ ] Category filter works
- [ ] Status filter works
- [ ] Pagination works
- [ ] Add product works
- [ ] All form tabs work
- [ ] Image upload works
- [ ] Multiple images work
- [ ] Bulk pricing saves
- [ ] Edit product works
- [ ] Duplicate product works
- [ ] Delete product works
- [ ] CSV upload validates
- [ ] CSV upload imports

### Order Management
- [ ] Orders list loads
- [ ] Status tabs filter correctly
- [ ] Date range filter works
- [ ] Search works
- [ ] Order details load
- [ ] Status update works
- [ ] Shipping info saves
- [ ] AWB tracking works
- [ ] Invoice generates
- [ ] Invoice downloads
- [ ] Cancel order works
- [ ] Stock restored on cancel
- [ ] Timeline displays correctly

### Inventory
- [ ] Stock list loads
- [ ] Low stock filter works
- [ ] Stock adjustment modal opens
- [ ] Add stock works
- [ ] Remove stock works
- [ ] Cannot go negative
- [ ] History logs correctly
- [ ] Reason saved

### Invoices
- [ ] Invoice list loads
- [ ] Invoice preview works
- [ ] PDF downloads correctly
- [ ] GST breakup correct
- [ ] Invoice email works

### Payments
- [ ] Payment list loads
- [ ] Filter by method works
- [ ] Manual payment recording works
- [ ] Credit ledger displays
- [ ] Outstanding calculates

### Pin Codes
- [ ] Pincode list loads
- [ ] Add pincode works
- [ ] Edit pincode works
- [ ] Delete pincode works
- [ ] Bulk upload works
- [ ] Serviceability check works

### Settings
- [ ] Company profile loads
- [ ] Company profile updates
- [ ] Tax settings work
- [ ] Admin users CRUD works
- [ ] Courier management works

---

## Buyer App Tests

### Authentication
- [ ] Login screen loads
- [ ] Phone validation works
- [ ] OTP request works
- [ ] OTP verification works
- [ ] Resend countdown works
- [ ] Registration form works
- [ ] GST validation works
- [ ] Registration submits
- [ ] Pending screen shows

### Home
- [ ] Home screen loads
- [ ] Categories display
- [ ] Featured products display
- [ ] Banner carousel works
- [ ] Recent orders show
- [ ] Cart badge updates
- [ ] Navigation works
- [ ] Pull to refresh works

### Categories
- [ ] Category list loads
- [ ] Subcategories expand
- [ ] Navigation to products works

### Product Listing
- [ ] Products load
- [ ] Category filter works
- [ ] Price filter works
- [ ] Brand filter works
- [ ] Sort works
- [ ] Pagination works
- [ ] Quick add to cart works
- [ ] MOQ enforced
- [ ] Out of stock indicated

### Product Details
- [ ] Details load
- [ ] Images display
- [ ] Image swipe works
- [ ] Pricing correct
- [ ] Bulk pricing shows
- [ ] Quantity selector works
- [ ] MOQ enforced
- [ ] Price updates with quantity
- [ ] Add to cart works
- [ ] Out of stock handled

### Cart
- [ ] Cart loads
- [ ] Items display correctly
- [ ] Quantity update works
- [ ] Remove item works
- [ ] Clear cart works
- [ ] Totals calculate correctly
- [ ] GST breakup correct
- [ ] Bulk discount applies
- [ ] Serviceability check works
- [ ] Minimum order enforced
- [ ] Proceed to checkout works

### Checkout
- [ ] Addresses load
- [ ] Add address works
- [ ] Select address works
- [ ] Serviceability shown
- [ ] Payment methods display
- [ ] UPI selection works
- [ ] COD selection works
- [ ] Credit selection works
- [ ] Insufficient credit blocked
- [ ] Order review correct
- [ ] Place order works
- [ ] UPI redirect works
- [ ] Order confirmation shows
- [ ] Cart cleared after order

### My Orders
- [ ] Orders list loads
- [ ] Status tabs work
- [ ] Order cards display
- [ ] Navigation to details works

### Order Details
- [ ] Details load
- [ ] Status correct
- [ ] Timeline displays
- [ ] Tracking info shows
- [ ] Invoice download works
- [ ] Reorder works
- [ ] Cancel works (when eligible)

### Profile
- [ ] Profile loads
- [ ] Edit profile works
- [ ] Addresses list
- [ ] Credit info correct
- [ ] Logout works

### Search
- [ ] Search opens
- [ ] Suggestions appear
- [ ] Recent searches save
- [ ] Search results load
- [ ] Product navigation works

### Notifications
- [ ] Notifications load
- [ ] Mark read works
- [ ] Navigation works
- [ ] Badge updates

---

## Integration Tests

### Complete Order Flow
- [ ] Register buyer
- [ ] Admin approves
- [ ] Buyer logs in
- [ ] Browse products
- [ ] Add to cart
- [ ] Checkout
- [ ] Place order (each payment type)
- [ ] Admin confirms order
- [ ] Invoice generated
- [ ] Admin ships order
- [ ] Tracking visible
- [ ] Admin marks delivered
- [ ] Order completed

### Credit Flow
- [ ] Admin sets credit limit
- [ ] Buyer sees available credit
- [ ] Buyer places credit order
- [ ] Credit deducted
- [ ] Ledger entry created
- [ ] Admin records payment
- [ ] Credit restored
- [ ] Ledger shows payment

### Stock Flow
- [ ] Product has stock
- [ ] Order reduces stock
- [ ] Low stock alert triggers
- [ ] Cancel restores stock
- [ ] Admin adjusts stock
- [ ] History logged

---

## Performance Tests

### API Response Times
- [ ] Login < 500ms
- [ ] Product list < 1s
- [ ] Order creation < 2s
- [ ] Dashboard load < 1s

### Mobile App
- [ ] Cold start < 3s
- [ ] Screen navigation < 300ms
- [ ] Image loading optimized
- [ ] List scrolling smooth

### Database
- [ ] Indexes used for queries
- [ ] No N+1 queries
- [ ] Large lists paginated

---

## Security Tests

### Authentication
- [ ] Passwords hashed
- [ ] JWT tokens secure
- [ ] Token expiry enforced
- [ ] Refresh tokens work
- [ ] Logout invalidates tokens

### Authorization
- [ ] Admin routes protected
- [ ] Role-based access works
- [ ] Buyer cannot access admin
- [ ] Buyers see only their data

### Input Validation
- [ ] All inputs validated
- [ ] SQL injection prevented
- [ ] XSS prevented
- [ ] File upload validated

### Rate Limiting
- [ ] OTP rate limited
- [ ] Login rate limited
- [ ] API rate limited

---

## Cross-Browser/Device Tests

### Admin Panel
- [ ] Chrome
- [ ] Firefox
- [ ] Safari
- [ ] Edge
- [ ] Responsive on tablet

### Buyer App
- [ ] Android 10+
- [ ] Various screen sizes
- [ ] Portrait mode
- [ ] Landscape mode (optional)

---

## Error Handling Tests

- [ ] Network error shows message
- [ ] API error shows message
- [ ] Form validation shows errors
- [ ] Retry options available
- [ ] Graceful degradation
- [ ] No sensitive data in errors
