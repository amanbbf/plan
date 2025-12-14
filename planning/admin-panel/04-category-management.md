# Admin Panel - Category Management Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Category Management |
| URL | `/admin/categories` |
| Access | Super Admin, Operations |
| Priority | P0 - Critical |
| Responsive | Desktop only |

---

## Purpose
Create and manage product categories and sub-categories for organizing the product catalog.

---

## Category Structure
```
Category (Level 1)
  └── Sub-category (Level 2)
       └── Products
```

**Example:**
```
Beverages
  ├── Cold Drinks
  ├── Juices
  └── Energy Drinks
Snacks
  ├── Chips
  ├── Biscuits
  └── Namkeen
```

---

## UI Components

### 1. Page Header
- Title: "Category Management"
- "Add Category" button (primary)

### 2. Category List (Left Panel - 40%)
| Column | Description |
|--------|-------------|
| Icon | Category icon/image |
| Name | Category name |
| Sub-categories | Count |
| Products | Total products |
| Status | Active/Inactive badge |
| Actions | Edit, Delete |

### 3. Sub-category Panel (Right Panel - 60%)
Appears when a category is selected.

**Header:**
- Selected category name
- "Add Sub-category" button

**Sub-category Table:**
| Column | Description |
|--------|-------------|
| Name | Sub-category name |
| Products | Product count |
| Status | Active/Inactive |
| Sort Order | Display order |
| Actions | Edit, Delete |

---

## Add/Edit Category Modal

### Form Fields
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Category Name | Text | Max 50 chars, unique | Yes |
| Description | Textarea | Max 200 chars | No |
| Image/Icon | File upload | JPG/PNG, max 500KB | No |
| Status | Toggle | - | Yes (default: Active) |
| Sort Order | Number | Positive integer | No |

---

## Add/Edit Sub-category Modal

### Form Fields
| Field | Type | Validation | Required |
|-------|------|------------|----------|
| Parent Category | Dropdown | - | Yes |
| Sub-category Name | Text | Max 50 chars, unique within parent | Yes |
| Description | Textarea | Max 200 chars | No |
| Image/Icon | File upload | JPG/PNG, max 500KB | No |
| Status | Toggle | - | Yes (default: Active) |
| Sort Order | Number | Positive integer | No |

---

## API Calls

### Categories

**GET `/api/admin/categories`**
Returns all categories with sub-category counts.

**POST `/api/admin/categories`**
Create new category.
```json
{
  "name": "Beverages",
  "description": "All types of drinks",
  "image": "url_or_base64",
  "status": "active",
  "sortOrder": 1
}
```

**PUT `/api/admin/categories/:id`**
Update category.

**DELETE `/api/admin/categories/:id`**
Delete category (only if no products linked).

### Sub-categories

**GET `/api/admin/categories/:id/subcategories`**
Returns sub-categories for a category.

**POST `/api/admin/subcategories`**
Create sub-category.
```json
{
  "categoryId": "cat_123",
  "name": "Cold Drinks",
  "description": "Carbonated beverages",
  "image": "url_or_base64",
  "status": "active",
  "sortOrder": 1
}
```

**PUT `/api/admin/subcategories/:id`**
Update sub-category.

**DELETE `/api/admin/subcategories/:id`**
Delete sub-category (only if no products linked).

---

## State Management

### Local State
- `categories` - Array of category objects
- `selectedCategory` - Currently selected category
- `subcategories` - Sub-categories of selected category
- `isLoading` - Loading states
- `showCategoryModal` - Add/edit category modal
- `showSubcategoryModal` - Add/edit sub-category modal
- `editingItem` - Item being edited (null for new)

---

## Business Rules

### Category Rules
1. Category name must be unique
2. Cannot delete category with products
3. Inactive category hides all its products from buyers
4. Sort order determines display sequence

### Sub-category Rules
1. Sub-category name unique within parent category
2. Cannot delete sub-category with products
3. Inactive sub-category hides products from buyers
4. Must belong to exactly one parent category

---

## Delete Confirmation

### Category with Sub-categories
> "This category has 5 sub-categories and 45 products. Are you sure you want to delete?"
> 
> Options: Cancel | Delete (disabled if has products)

### Empty Category
> "Delete category 'Beverages'?"
> 
> Options: Cancel | Delete

### Sub-category with Products
> "This sub-category has 12 products. Please reassign products before deleting."
>
> Options: OK (close dialog)

---

## Drag & Drop (Optional in MVP)
- Reorder categories by dragging
- Reorder sub-categories within category
- Updates sort order automatically

---

## Design Specifications

### Layout
- Two-panel layout (40% / 60%)
- Categories panel has fixed width
- Sub-category panel expands

### Visuals
- Category icons: 48x48px
- Cards with subtle shadow
- Hover effects on rows
- Draggable indicators

### Status Colors
- Active: Green badge
- Inactive: Gray badge

---

## Empty States

### No Categories
"No categories found. Click 'Add Category' to create your first category."

### No Sub-categories
"No sub-categories in [Category Name]. Click 'Add Sub-category' to create one."

### Category Not Selected
"Select a category from the left to view its sub-categories."

---

## Validation Messages

| Scenario | Message |
|----------|---------|
| Duplicate name | "Category with this name already exists" |
| Empty name | "Category name is required" |
| Invalid image | "Please upload a valid image (JPG/PNG)" |
| Delete with products | "Cannot delete. Move products first." |

---

## Checklist

### Development
- [ ] Create two-panel layout
- [ ] Build category list
- [ ] Build sub-category list
- [ ] Create add category modal
- [ ] Create edit category modal
- [ ] Create add sub-category modal
- [ ] Create edit sub-category modal
- [ ] Implement image upload
- [ ] Add delete with confirmation
- [ ] Implement status toggle
- [ ] Add sort order functionality
- [ ] Connect all APIs

### Testing
- [ ] Add category works
- [ ] Edit category works
- [ ] Delete empty category works
- [ ] Delete protected category blocked
- [ ] Add sub-category works
- [ ] Edit sub-category works
- [ ] Delete sub-category works
- [ ] Image upload works
- [ ] Status toggle works
- [ ] Sort order updates

---

## Dependencies
- Image upload service
- Modal component
- Confirmation dialog component
- File upload component
- Categories API
