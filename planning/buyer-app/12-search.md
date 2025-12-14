# Buyer App - Search Page

## Page Info
| Attribute | Value |
|-----------|-------|
| Page Name | Search |
| Route | `/search` or `SearchScreen` |
| Access | Authenticated buyers |
| Priority | P0 - Critical |
| Platform | Mobile App (Expo) + Web |

---

## Purpose
Allow buyers to quickly find products by name, brand, or category with auto-suggestions and search history.

---

## UI Components

### 1. Search Header
```
┌─────────────────────────────────────┐
│ [←] [ Search products...      ] [X] │
└─────────────────────────────────────┘
```

### 2. Initial State (before typing)

**Recent Searches**
```
Recent Searches        [Clear All]
├── parle g biscuit
├── cold drinks
└── tata salt
```

**Popular Searches**
```
Popular
├── Biscuits
├── Cooking Oil
├── Rice
└── Sugar
```

### 3. Auto-Suggestions (while typing)
```
"par" →
┌─────────────────────────────────────┐
│ 🔍 parle g biscuit                  │
│ 🔍 parle hide and seek              │
│ 🔍 parle monaco                     │
│ ─────────────────────               │
│ 📦 Parle-G 800g Pack                │
│ 📦 Parle Hide & Seek 400g           │
└─────────────────────────────────────┘
```

### 4. Search Results
Same as Product Listing Page with search context.

### 5. No Results
```
┌─────────────────────────────────────┐
│     😕                              │
│     No products found for           │
│     "xyz123"                        │
│                                     │
│     Try different keywords          │
│     or browse categories            │
│                                     │
│     [Browse Categories]             │
└─────────────────────────────────────┘
```

---

## Search Features

### Auto-Suggestions
- Show after 2+ characters
- Debounce: 300ms
- Mix of:
  - Search terms (recent + popular)
  - Matching products (top 5)
  - Matching categories

### Recent Searches
- Store locally
- Last 10 searches
- Tap to search again
- Swipe/tap to remove

### Popular Searches
- From API (trending)
- Updated periodically
- Based on all users

---

## API Calls

### GET `/api/search/suggestions`
Get auto-suggestions.

**Query:** `q=par`

**Response:**
```json
{
  "terms": ["parle g", "parle hide seek"],
  "products": [
    { "id": "prod_1", "name": "Parle-G 800g", "image": "url", "price": 42 }
  ],
  "categories": [
    { "id": "cat_1", "name": "Biscuits" }
  ]
}
```

### GET `/api/search/popular`
Get popular search terms.

### GET `/api/products?search=...`
Search products (same as listing API).

---

## State Management

### Local State
- `query` - Search input
- `suggestions` - Auto-suggestions
- `results` - Search results
- `recentSearches` - From local storage
- `isLoading` - Loading state

### Local Storage
- Recent searches array
- Max 10 items
- Most recent first

---

## Search Flow

```
1. Open search screen
2. Show recent & popular searches
3. Start typing
4. After 2 chars, fetch suggestions
5. Show suggestions (debounced)
6. Tap suggestion or press search
7. Save to recent searches
8. Show search results
9. Can filter/sort results
```

---

## Voice Search (Future)

- Microphone button in search bar
- Tap to speak
- Convert speech to text
- Auto-search

---

## Barcode/QR Scan (Future)

- Camera button in search bar
- Scan product barcode
- Direct to product if found

---

## Filter & Sort in Results

Same as Product Listing:
- Sort by relevance, price, name
- Filter by category, brand, price

---

## Design Specifications

### Search Bar
- Full width with padding
- Auto-focus on open
- Clear button (X) when has text
- Back button to close

### Suggestions List
- Icon prefix (🔍 for terms, 📦 for products)
- Highlight matching text
- Tap to select
- Keyboard-navigable

### Recent Searches
- List with remove option
- "Clear All" action
- Gray text

### No Results
- Centered layout
- Emoji/illustration
- Helpful message
- Action button

---

## Accessibility

- [ ] Voice search label
- [ ] Clear button accessible
- [ ] Keyboard navigation
- [ ] Screen reader support

---

## Checklist

### Development
- [ ] Create search screen
- [ ] Implement search input
- [ ] Show recent searches
- [ ] Show popular searches
- [ ] Implement auto-suggestions
- [ ] Debounce suggestions
- [ ] Save to recent searches
- [ ] Display search results
- [ ] Handle no results
- [ ] Clear search history
- [ ] Navigate to product

### Testing
- [ ] Search input works
- [ ] Suggestions appear
- [ ] Debounce working
- [ ] Recent searches save
- [ ] Results display
- [ ] No results handled
- [ ] Product navigation works

---

## Dependencies
- Search API
- Local storage (AsyncStorage)
- Debounce utility
- Product listing component
