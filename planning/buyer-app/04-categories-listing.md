# Buyer App - Categories Listing Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Categories |
| Route | `/categories` or `CategoriesScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Display all product categories and sub-categories for easy navigation to products.

---

## UI Components

### 1. Header
- Back button
- Title: "Categories"
- Search icon

### 2. Categories List

**Option A: Expandable List**
```
▶ Beverages
  ├── Cold Drinks
  ├── Juices
  └── Energy Drinks
▶ Snacks
  ├── Chips
  ├── Biscuits
  └── Namkeen
```

**Option B: Two-Column Grid**
- Category cards with icon/image
- Category name
- Product count badge
- Tap to see sub-categories

### 3. Sub-category View
When category selected:
- Category name as header
- Sub-categories as list/grid
- Each showing product count

---

## Category Card Component

```
┌─────────────────────┐
│    [Icon/Image]     │
│                     │
│     Beverages       │
│     45 products     │
└─────────────────────┘
```

---

## Data Structure

```json
{
  "categories": [
    {
      "id": "cat_beverages",
      "name": "Beverages",
      "icon": "url",
      "image": "url",
      "productCount": 45,
      "subcategories": [
        {
          "id": "subcat_colddrinks",
          "name": "Cold Drinks",
          "productCount": 20
        },
        {
          "id": "subcat_juices",
          "name": "Juices",
          "productCount": 15
        }
      ]
    }
  ]
}
```

---

## API Calls

### GET `/api/categories`
Get all categories with sub-categories.

**Response:**
```json
{
  "success": true,
  "data": {
    "categories": [...]
  }
}
```

### GET `/api/categories/:id`
Get single category with sub-categories.

---

## User Flow

```
1. Open Categories screen
2. See all categories
3. Tap on a category
4. See sub-categories expand/navigate
5. Tap on sub-category
6. Navigate to product listing
```

---

## Navigation Patterns

### Pattern A: Expand in place
- Tap category to expand
- Show sub-categories inline
- Tap sub-category to go to products

### Pattern B: Navigate to sub-page
- Tap category to navigate
- New screen with sub-categories
- Tap sub-category to go to products

### Recommended for MVP
Pattern A (expand in place) - simpler, faster

---

## State Management

### Local State
- `categories` - Array of categories
- `expandedCategory` - Currently expanded (for Pattern A)
- `isLoading` - Loading state

---

## Empty States

### No Categories
"No categories available yet. Check back soon!"

### Network Error
"Unable to load categories. Tap to retry."

---

## Design Specifications

### Layout
- Full screen list/grid
- Safe area padding
- Pull to refresh

### Grid (if used)
- 2 columns on phone
- 3 columns on tablet
- Equal spacing

### List (if used)
- Full width rows
- 60px row height
- Chevron icon for expandable
- Indent for sub-categories

### Category Card
- Icon/image: 60x60px
- Name: Bold, 16px
- Count: Light, 14px

### Colors
- Category background: Light brand color or white
- Sub-category: Slightly indented, lighter

---

## Checklist

### Development
- [ ] Create categories screen
- [ ] Fetch categories from API
- [ ] Display category list/grid
- [ ] Implement expand/collapse
- [ ] Show sub-categories
- [ ] Navigate to products on tap
- [ ] Add loading state
- [ ] Add empty state
- [ ] Add error handling
- [ ] Add pull to refresh

### Testing
- [ ] Categories load correctly
- [ ] Expand/collapse works
- [ ] Sub-categories display
- [ ] Navigation to products works
- [ ] Product counts accurate
- [ ] Empty state shows

---

## Dependencies
- Navigation library
- Categories API
- Image caching
- Accordion/expandable component
