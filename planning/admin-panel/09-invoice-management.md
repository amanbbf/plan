# Admin Panel - Invoice Management Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Invoice Management |
| URL | `/admin/invoices` |
| Access | Super Admin, Finance |
| Priority | P0 - Critical |
| Responsive | Desktop only |

---

## Purpose
View, generate, download, and manage GST-compliant invoices for all orders.

---

## UI Components

### 1. Page Header
- Title: "Invoices"
- Search bar (invoice number, order ID)
- Date range filter
- "Export All" button

### 2. Quick Stats
| Stat | Description |
|------|-------------|
| Total Invoices | Count this month |
| Total Value | Sum of invoice amounts |
| GST Collected | Total tax collected |
| Pending | Unpaid invoices |

### 3. Invoice Table
| Column | Description | Sortable |
|--------|-------------|----------|
| Invoice No. | INV-YYYY-NNNN | Yes |
| Order ID | Linked order | Yes |
| Date | Invoice date | Yes |
| Buyer | Business name + GST | No |
| Amount | Total amount | Yes |
| GST | Tax amount | Yes |
| Status | Paid/Unpaid | Yes |
| Actions | View, Download, Send | No |

### 4. Filter Options
- Date range
- Payment status
- Amount range

---

## Invoice View Modal

### Invoice Preview
Full GST-compliant invoice display:

**Header Section**
- Company logo
- Company name & address
- GSTIN
- Invoice number
- Invoice date
- Due date (if credit)

**Buyer Section**
- Business name
- Address
- GSTIN
- Phone

**Items Table**
| # | Description | HSN | Qty | Rate | Taxable | GST% | GST Amt | Total |
|---|-------------|-----|-----|------|---------|------|---------|-------|
| 1 | Product name | 1905 | 24 | 42 | 1008 | 5% | 50.40 | 1058.40 |

**Summary Section**
```
Subtotal:           ₹5,000.00
CGST (2.5%):        ₹  125.00
SGST (2.5%):        ₹  125.00
(or IGST if inter-state)
Delivery:           ₹   50.00
Discount:          -₹  100.00
---------------------------------
Grand Total:        ₹5,200.00
```

**Footer Section**
- Payment terms
- Bank details (for credit orders)
- Signature line
- Terms & conditions

---

## Invoice Number Format

### Format: `INV-YYYY-NNNNNN`
- YYYY = Financial year start (e.g., 2024)
- NNNNNN = Sequential number (000001, 000002...)

### Reset Policy
- Resets each financial year (April 1)
- Auto-increments per invoice

### Examples
- INV-2024-000001
- INV-2024-000125

---

## GST Compliance

### Required Fields
- Seller GSTIN
- Buyer GSTIN (if registered)
- HSN codes for all products
- Tax breakup (CGST/SGST or IGST)
- Invoice date
- Place of supply

### Tax Calculation
```
If seller_state == buyer_state:
    Apply CGST + SGST (equal split)
Else:
    Apply IGST (full rate)
```

### GST Rates
- 0% - Exempt items
- 5% - Basic food items
- 12% - Processed foods
- 18% - Standard rate
- 28% - Luxury items

---

## API Calls

### GET `/api/admin/invoices`
List all invoices with filters.

### GET `/api/admin/invoices/:id`
Get invoice details.

### GET `/api/admin/invoices/:id/pdf`
Download invoice as PDF.

### POST `/api/admin/invoices/:id/send`
Email invoice to buyer.

### POST `/api/admin/invoices/:id/regenerate`
Regenerate invoice (if order changed).

### GET `/api/admin/invoices/stats`
Invoice statistics.

### GET `/api/admin/invoices/gst-report`
GST report for filing (monthly).

---

## Invoice Data Structure

```json
{
  "_id": "inv_123",
  "invoiceNumber": "INV-2024-000125",
  "order": "order_123",
  "seller": {
    "name": "ABC Wholesale",
    "address": "...",
    "gstin": "29AABCU9603R1ZM",
    "state": "Karnataka",
    "stateCode": "29"
  },
  "buyer": {
    "name": "Krishna Stores",
    "address": "...",
    "gstin": "29AABCU9603R1ZM",
    "state": "Karnataka",
    "stateCode": "29"
  },
  "items": [
    {
      "name": "Parle-G 800g",
      "hsn": "1905",
      "quantity": 24,
      "rate": 42,
      "taxableAmount": 1008,
      "gstRate": 5,
      "cgst": 25.20,
      "sgst": 25.20,
      "igst": 0,
      "total": 1058.40
    }
  ],
  "summary": {
    "subtotal": 5000,
    "cgst": 125,
    "sgst": 125,
    "igst": 0,
    "deliveryCharge": 50,
    "discount": 100,
    "grandTotal": 5200
  },
  "invoiceDate": "2024-01-15",
  "dueDate": "2024-02-15",
  "paymentStatus": "paid",
  "pdfUrl": "...",
  "createdAt": "..."
}
```

---

## PDF Generation

### Template Requirements
- Clean, professional design
- Print-ready (A4 size)
- All GST fields included
- Company branding
- QR code (optional for verification)

### PDF Library Options
- Puppeteer (HTML to PDF)
- PDFKit
- jsPDF

---

## Email Invoice

### Email Content
```
Subject: Invoice #INV-2024-000125 from [Company Name]

Dear [Buyer Name],

Please find attached invoice for your recent order.

Invoice Number: INV-2024-000125
Order Number: ORD-2024-0001
Amount: ₹5,200.00
Due Date: 15-Feb-2024

[Download Invoice]

Thank you for your business!

[Company Name]
[Contact Details]
```

---

## GST Reports

### Monthly Summary
| GST Rate | Taxable Amount | CGST | SGST | IGST |
|----------|----------------|------|------|------|
| 5% | 50,000 | 1,250 | 1,250 | 0 |
| 12% | 30,000 | 1,800 | 1,800 | 0 |
| 18% | 20,000 | 1,800 | 1,800 | 0 |
| **Total** | **1,00,000** | **4,850** | **4,850** | **0** |

### Export Options
- CSV
- Excel
- PDF

---

## Checklist

### Development
- [ ] Create invoice list page
- [ ] Build invoice view modal
- [ ] Implement PDF generation
- [ ] Add download functionality
- [ ] Create email invoice feature
- [ ] Build GST report generator
- [ ] Add search and filters
- [ ] Implement auto-generation on order confirm
- [ ] Add invoice number sequencing

### Testing
- [ ] Invoice list displays correctly
- [ ] PDF generates properly
- [ ] All GST fields present
- [ ] CGST/SGST vs IGST logic works
- [ ] Download works
- [ ] Email delivery works
- [ ] Invoice numbering sequential
- [ ] GST report accurate

---

## Dependencies
- PDF generation library
- Email service
- Order management system
- GST calculation utility
- File storage (for PDFs)
