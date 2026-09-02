# SplitPlate — System Architecture

> **Version:** 1.0.0 | **Last updated:** August 2025 | **Authors:** Jishu, Anya, Akankshya

---

## 1. Overview

SplitPlate is a group food ordering and bill-splitting web application. It solves a real gap in platforms like Swiggy — the inability for a group to order from multiple restaurants in one session, auto-split the bill per person, and track payments in real time.

The system is built as **three standalone HTML files** sharing a single **Firebase Firestore** database, requiring no server, no build step, and no framework.

```
┌─────────────────────────────────────────────────────────┐
│                   SPLITPLATE ECOSYSTEM                  │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ swiggy-      │  │ restaurant-  │  │ splitplate-  │  │
│  │ split.html   │  │ dashboard    │  │ analytics    │  │
│  │              │  │ .html        │  │ .html        │  │
│  │  (Customer)  │  │ (Restaurant) │  │  (Business)  │  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  │
│         │                 │                  │           │
│         └─────────────────┼──────────────────┘           │
│                           │                              │
│              ┌────────────▼────────────┐                 │
│              │   Firebase Firestore    │                 │
│              │   (Cloud Database)      │                 │
│              └─────────────────────────┘                 │
└─────────────────────────────────────────────────────────┘
```

---

## 2. Tech Stack

| Layer | Technology | Reason |
|---|---|---|
| Frontend | Vanilla HTML/CSS/JS | No framework needed, single-file deployment |
| Database | Firebase Firestore (NoSQL) | Real-time, serverless, free tier sufficient |
| Authentication | Custom Firestore auth | Email + password stored and cross-verified manually |
| Fonts | Google Fonts (Inter, Playfair Display) | Design quality |
| Charts | Pure CSS/SVG (no library) | Zero dependencies |
| Hosting | Local file / any static host | Open via Chrome Ctrl+O |

---

## 3. Application Files

### 3.1 `swiggy-split.html` — Customer App
The main ordering interface. 4 screens navigated without page reload:

```
Screen 0: Login     → Sign In / Sign Up
Screen 1: Home      → Restaurant list + Offers banner
Screen 2: Menu      → Items, mood filter, person tagging
Screen 3: Cart/Pay  → Bill split, payments, QR, support
```

### 3.2 `restaurant-dashboard.html` — Restaurant View
Analytics portal for each of the 15 restaurants. Login: select restaurant → view orders.

### 3.3 `splitplate-analytics.html` — Business View
Admin-level analytics across the entire platform. Login: password `splitplate2025`.

---

## 4. Database Architecture

### 4.1 Firestore Structure

```
Firestore (default)
│
└── users/                              ← Collection
    └── {emailKey}/                     ← Document (e.g. jishu_at_gmail_com)
        ├── uid:          "jishu_at_gmail_com"
        ├── name:         "Jishu"
        ├── email:        "jishu@gmail.com"
        ├── password:     "pass123"          ← Plain text (demo only)
        ├── createdAt:    "2025-08-01T..."
        ├── lastLogin:    "2025-08-12T..."
        │
        └── orders/                     ← Subcollection
            └── {orderId}/              ← Document (e.g. order-1784265451526)
                ├── orderId
                ├── userEmail
                ├── userName
                ├── grandTotal
                ├── subtotal
                ├── deliveryFee:     40
                ├── platformFee:     8
                ├── tax
                ├── itemCount
                ├── orderState:      "building" | "buffer" | "sent"
                ├── billPaidByOrganizer: true/false
                ├── restaurants:     ["Meghna Foods", "Corner House"]
                ├── restaurantName:  "Meghna Foods + Corner House"
                ├── cart:            [Array of items]
                ├── cartByRestaurant: { "Meghna Foods": {items, subtotal} }
                ├── people:          ["You", "Riya", "Karan"]
                ├── payState:        { "Riya": {status:"qr_sent"} }
                ├── refundState:     { "Riya": {refundAmt: 50} }
                ├── perPersonDetails: { "Riya": {items, totalOwed, tax, ...} }
                └── updatedAt
```

### 4.2 Email Key Convention
Firebase document IDs cannot contain `.` or `@`. Emails are converted:
```
jishu@gmail.com  →  jishu_at_gmail_com
```

### 4.3 Why Subcollections?
Each user's orders live under `users/{uid}/orders/` (not a root collection) so:
- Data is scoped per user — no cross-user data leakage
- Firestore security rules can be applied per user path
- The analytics dashboard loops all users and aggregates from subcollections

---

## 5. Authentication Flow

```
Sign Up:
User fills name/email/password
        ↓
Check if emailKey doc exists in Firestore
        ↓ (not found)
Create users/{emailKey} document
        ↓
Set currentUser in memory → go to Home

Sign In:
User fills email/password
        ↓
Fetch users/{emailKey} from Firestore
        ↓ (found)
Compare password field
        ↓ (match)
Update lastLogin timestamp → go to Home
```

> **Note:** This is a demo authentication system. In production, use Firebase Authentication SDK with hashed passwords.

---

## 6. Bill Split Logic

```
For each person P in the order:

  itemTotal(P)  = sum of prices of items tagged to P
  orderingCount = number of people who have ≥1 item
  sharedFees(P) = (deliveryFee + platformFee) / orderingCount
  tax(P)        = itemTotal(P) × 5%
  totalOwed(P)  = itemTotal(P) + sharedFees(P) + tax(P)

Key rule: if a person has NO items → they pay ₹0 (no shared fees)
```

Example with 3 people, only 2 ordering:
```
You:   ₹340 items + ₹24 fees + ₹17 tax = ₹381
Riya:  ₹160 items + ₹24 fees + ₹8 tax  = ₹192
Karan: ₹0   items + ₹0  fees + ₹0 tax  = ₹0
```

---

## 7. Multi-Restaurant Multi-Delivery Flow

```
User adds items from Meghna Foods + Corner House
                ↓
Cart groups items by restaurant
                ↓
User pays full bill (organiser)
                ↓
User confirms order ("I'm sure")
                ↓
confirmOrder() groups cart by restaurant
                ↓
For each unique restaurant → assign random delivery partner
                ↓
Show Delivery 1 (Meghna) + Delivery 2 (Corner House)
with simultaneous ETAs
```

---

## 8. Data Flow Diagram

```
[User adds item to cart]
        ↓
addMenuItem() / changeQty()
        ↓
recalcTotals() — updates totals, per-person split
        ↓
saveToFirebase() — debounced 1000ms
        ↓
Firestore: users/{uid}/orders/{orderId}.set({...})
        ↓
Firebase Console shows live data
```

---

## 9. Settings & Theming

Three themes stored in `localStorage`:

| Theme | Primary | Success | Error |
|---|---|---|---|
| Dark (default) | `#FF6A2B` | `#34d399` | `#f87171` |
| Light | `#e8521a` | `#16a34a` | `#dc2626` |
| Colour Blind | `#4A90D9` | `#F5A623` | `#F5A623` |

Colour Blind mode (Deuteranopia-optimised):
- Replaces all green/red with blue/amber
- Adds text prefix (`✓` / `⚠`) to all status indicators so colour is never the only signal

---

## 10. Restaurants (15 total)

| # | Name | Cuisine |
|---|---|---|
| 1 | California Burrito | Mexican |
| 2 | Meghna Foods | Biryani |
| 3 | Anitha's Food Cafe | North Indian |
| 4 | Small Mingos | Burgers |
| 5 | Big Mingos | BBQ & Grill |
| 6 | Nandini | Tea, Coffee & Tiffin |
| 7 | Corner House | Ice Cream |
| 8 | Taco Bell | Mexican Fast Food |
| 9 | Uru Brewpark | Continental |
| 10 | Norwa Chai | Chai & Street Food |
| 11 | Empire Restaurant | North Indian |
| 12 | Belgian Waffles | Desserts |
| 13 | Howlers Food Truck | Street Food Fusion |
| 14 | Smash Guys | Smash Burgers |
| 15 | Rameshwaram Cafe | South Indian |

---

## 11. Security Considerations (Demo Scope)

| Item | Current (Demo) | Production Recommendation |
|---|---|---|
| Password storage | Plain text in Firestore | Firebase Auth + bcrypt hashing |
| Firestore rules | `allow read, write: if true` | Per-user rules with auth token |
| Admin dashboard | Password in JS source | Server-side session auth |
| API key | Exposed in client HTML | Restrict key by domain in GCP console |

---

*SplitPlate · Christ (Deemed to be University) · B.Sc. Economics & Data Science · CIA Assessment 2025*
