# Buyer App - Product Listing Page (PLP)

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Product Listing |
| Route | `/products` or `ProductListScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Display products in a category/sub-category with search, filter, and sort capabilities. Allow quick add to cart.

---

## Entry Points
- From category/sub-category selection
- From search results
- From "View All" links
- From banner/promotion deep links

---

## UI Components

### 1. Header
- Back button
- Category name as title
- Search icon
- Filter icon with active indicator

### 2. Sub-navigation (if from category)
- Horizontal scrollable chips
- "All" + sub-categories
- Active chip highlighted

### 3. Sort & Filter Bar
- Sort dropdown: "Sort by: Price (Low-High)"
- Filter button: Opens filter sheet

### 4. Product Grid/List
- Grid view (default): 2 columns
- List view option
- Product cards

### 5. Product Card (Grid)
```
┌────────────────┐
│   [Product     │
│    Image]      │
│                │
│ Product Name   │
│ ₹42  ₹50       │
│ MOQ: 12 packs  │
│ [  Add  ]      │
└────────────────┘
```

### 6. Product Card (List)
```
┌──────────────────────────────────┐
│ [Img] │ Product Name             │
│       │ ₹42  ₹50 (16% off)       │
│       │ MOQ: 12 packs            │
│       │ [Qty: -  12  +] [Add]    │
└──────────────────────────────────┘
```

### 7. Bottom Cart Summary
- When items in cart
- "X items | ₹XXX" 
- "View Cart" button

---

## Filter Options

### Filter Sheet (Bottom)
| Filter | Type |
|--------|------|
| Price Range | Range slider |
| Brand | Multi-select checkboxes |
| In Stock | Toggle |
| Discount | Toggle (has discount) |

### Sort Options
- Relevance (default)
- Price: Low to High
- Price: High to Low
- Newest First
- Name: A to Z

---

## API Calls

### GET `/api/products`
List products with filters.

**Query Params:**
- `category` - Category ID
- `subcategory` - Sub-category ID
- `search` - Search term
- `minPrice`, `maxPrice` - Price range
- `brand` - Brand filter
- `inStock` - true/false
- `hasDiscount` - true/false
- `sortBy` - field name
- `sortOrder` - asc/desc
- `page`, `limit` - Pagination

**Response:**
```json
{
  "success": true,
  "data": {
    "products": [
      {
        "id": "prod_1",
        "name": "Parle-G 800g",
        "sku": "BIS-PG-800",
        "image": "url",
        "mrp": 50,
        "price": 42,
        "discount": 16,
        "gstRate": 5,
        "moq": 12,
        "increment": 6,
        "unit": "pack",
        "stock": 500,
        "inStock": true,
        "bulkPricing": [...]
      }
    ],
    "pagination": {
      "total": 78,
      "page": 1,
      "limit": 20,
      "pages": 4
    },
    "filters": {
      "brands": ["Parle", "Britannia", "ITC"],
      "priceRange": { "min": 10, "max": 500 }
    }
  }
}
```

---

## State Management

### Local State
- `products` - Product array
- `pagination` - Pagination state
- `filters` - Active filters
- `sortBy` - Current sort
- `isLoading` - Loading state
- `isRefreshing` - Pull refresh state
- `viewMode` - grid/list

### Filter State
```javascript
{
  category: "cat_123",
  subcategory: null,
  search: "",
  minPrice: null,
  maxPrice: null,
  brands: [],
  inStock: false,
  hasDiscount: false
}
```

---

## Add to Cart Flow

### Quick Add (Grid view)
1. Tap "Add" button
2. Add MOQ quantity
3. Show toast confirmation
4. Update bottom cart summary

### Quantity Selector (List view)
1. Quantity starts at MOQ
2. +/- buttons adjust by increment
3. Cannot go below MOQ
4. Tap "Add" to add to cart

### MOQ Display
- "Min order: 12 packs"
- Quantity always >= MOQ
- Increment by defined step

---

## Bulk Pricing Indicator

If product has bulk pricing:
- Show "Bulk discount available" tag
- On tap, show pricing tiers:
  ```
  24+ packs: 5% off
  48+ packs: 8% off
  ```

---

## Out of Stock Handling

- Show "Out of Stock" badge
- Disable Add button
- Gray out card slightly
- Option: "Notify when available"

---

## Infinite Scroll / Load More

- Load 20 products initially
- Load more on scroll near bottom
- Show loading indicator
- "No more products" when end reached

---

## Empty States

### No Products Found
"No products found matching your filters. Try adjusting filters."
- Clear Filters button

### No Products in Category
"No products in this category yet."

### Network Error
"Unable to load products. Tap to retry."

---

## Search Flow

When search icon tapped:
1. Open search screen/overlay
2. Show recent searches
3. Type to search
4. Results appear as typing
5. Tap product or "See all results"

---

## Design Specifications

### Layout
- Header: Fixed 56px
- Sub-nav: 48px (if present)
- Sort/Filter bar: 48px
- Grid: 2 columns, 8px gap
- Bottom cart: 60px (when visible)

### Product Card (Grid)
- Width: 50% - 12px
- Image: Square, fill width
- Padding: 12px
- Border radius: 8px
- Shadow: subtle

### Prices
- Selling price: Bold, 16px, dark
- MRP: Strikethrough, 14px, gray
- Discount: Green badge

### Filters
- Bottom sheet: 60% height
- Scroll if many options
- "Apply" button sticky at bottom

---

## Performance

- Lazy load images
- Virtualized list for many products
- Debounce filter changes
- Cache product data

---

## Checklist

### Development
- [ ] Create product list screen
- [ ] Implement product grid
- [ ] Create product card component
- [ ] Add sub-category chips
- [ ] Implement sorting
- [ ] Build filter sheet
- [ ] Add price range filter
- [ ] Add brand filter
- [ ] Implement pagination/infinite scroll
- [ ] Add to cart functionality
- [ ] Quantity selector
- [ ] MOQ enforcement
- [ ] Bottom cart summary
- [ ] Empty states
- [ ] Loading states
- [ ] Search integration

### Testing
- [ ] Products load correctly
- [ ] Filters work properly
- [ ] Sorting works
- [ ] Add to cart works
- [ ] MOQ enforced
- [ ] Pagination works
- [ ] Empty states show
- [ ] Out of stock handled

---

## Dependencies
- Product API
- Cart state management
- Filter sheet component
- Infinite scroll/pagination
- Image caching
- Toast notifications
