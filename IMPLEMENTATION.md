# SplitPlate — Implementation Guide

> **Version:** 1.0.0 | **Last updated:** August 2025 | **Authors:** Jishu, Anya

---

## 1. Project Structure

```
splitplate/
├── swiggy-split.html          ← Main customer app (~3900 lines)
├── restaurant-dashboard.html  ← Restaurant analytics portal
├── splitplate-analytics.html  ← Business intelligence dashboard
├── ARCHITECTURE.md            ← System design document
└── IMPLEMENTATION.md          ← This file
```

All three files are **self-contained** — CSS, JavaScript, and HTML in one file each. No build tools, no npm, no bundler.

---

## 2. How to Run

### Step 1 — Open in Chrome
```
Chrome → Ctrl+O → select swiggy-split.html → Open
```
Do NOT use VS Code Live Server (blocks Firebase CDN in some configs).

### Step 2 — Firebase Rules
Go to **Firebase Console → Firestore Database → Rules** → paste and publish:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

### Step 3 — First use
Click **Sign Up** → enter name, email, password → you're in.

---

## 3. Firebase Configuration

All three files share the same Firebase project config:

```javascript
const firebaseConfig = {
  apiKey:            "AIzaSyD7Jmssw6_DB2nCGzzltzhGyGVCBOMPLcw",
  authDomain:        "splitplat.firebaseapp.com",
  projectId:         "splitplat",
  storageBucket:     "splitplat.firebasestorage.app",
  messagingSenderId: "964172883823",
  appId:             "1:964172883823:web:b0a56c31d5fdb1fbac6a9a"
};
```

SDK loaded via CDN (no npm required):
```html
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore-compat.js"></script>
```

---

## 4. Core Functions — `swiggy-split.html`

### 4.1 Screen Navigation
```javascript
function goScreen(name)
// name: 'home' | 'menu' | 'cart'
// Shows target screen, hides others, calls relevant render functions
```

### 4.2 Restaurant Selection
```javascript
function selectResto(restoId)
// Sets selectedRestoId, renders menu items for that restaurant
// Triggers mood filter reset and menu re-render
```

### 4.3 Adding Items
```javascript
function addMenuItem(restoId, i)
// i = index in restaurant's menu array
// Checks for existing same item+person in cart → increments qty
// Otherwise pushes new cart entry with basePrice/baseCal for qty scaling
```

### 4.4 Quantity Control
```javascript
function changeQty(cartIndex, delta)
// delta: +1 or -1
// Updates qty, price = basePrice × qty, cal = baseCal × qty
// Removes item if qty reaches 0
```

### 4.5 Bill Calculation
```javascript
function personAmount(person)
// Returns: itemTotal + sharedFees + tax
// sharedFees only charged if person has ≥1 item (peopleOrdering)
// Returns 0 if person has no items

function recalcTotals()
// Called after every cart change
// Updates: header totals, bill section, per-person cards, menu total box
// Debounced Firebase save triggered here
```

### 4.6 Payment Flow
```javascript
function payFullBillNow()
// Shows "Are you sure?" confirmation modal with grand total
// On confirm → calls confirmFullPayment()

function confirmFullPayment()
// Sets billPaidByOrganizer = true
// Triggers startBufferTimer() → shows buffer/review panel

function confirmOrder()
// Groups cart by restaurant
// Assigns random delivery partner per restaurant
// Shows multi-delivery cards (Delivery 1, Delivery 2...)
// Calls saveFinalOrder() to update Firebase
```

### 4.7 Firebase Save
```javascript
window.saveToFirebase = function()
// Debounced 1000ms — prevents excessive writes
// Saves full order state including perPersonDetails breakdown
// Path: users/{uid}/orders/{currentOrderId}
// Called from: addMenuItem, changeQty, payFullBillNow, recalcTotals

window.saveFinalOrder = async function()
// Sets orderState: 'sent' and confirmedAt timestamp
// Called from confirmOrder()
```

### 4.8 Add Person by Email
```javascript
// addPersonBtn click handler:
window.fetchPersonByEmail(email)
// Looks up users/{emailKey} in Firestore
// Returns {name, email} if found, null if not
// User's name is added to people[] array
```

---

## 5. Key State Variables

These live in the main `<script>` block (global scope):

```javascript
// Order state
let cart = []              // [{item, price, cal, basePrice, baseCal, qty, person, resto, restoId}]
let people = ['You']       // Names of people in this group order
let payState = {}          // {personName: {status: 'paid'|'qr_sent'|'unpaid', time}}
let refundState = {}       // {personName: {refundAmt, refundAt}}
let billPaidByOrganizer = false
let billPaidTime = null
let selectedRestoId = null
let orderState = 'unpaid'  // 'unpaid' | 'buffer' | 'sent'

// Constants
const ORGANIZER = 'You'
const DELIVERY_FEE = 40    // ₹
const PLATFORM_FEE = 8     // ₹
const TAX_RATE = 0.05      // 5%
const CALORIE_BUDGET = 2000

// Firebase (in second <script> block)
var db = null
var currentUser = null     // {name, email, uid}
var currentOrderId = 'order-' + Date.now()
```

---

## 6. RESTAURANTS Array Structure

Each restaurant entry:
```javascript
{
  id: 'meghna',
  name: 'Meghna Foods',
  emoji: '🍛',
  cuisine: 'Biryani · Kebabs',
  rating: 4.6,
  time: '25-35 min',
  badge: 'popular',          // 'popular' | 'new' | null
  desc: 'Authentic Hyderabadi...',
  menu: [
    {
      name: 'Chicken Biryani',
      price: 240,
      cal: 780,
      mood: ['hungry']       // 'all' | 'hungry' | 'lazy' | 'healthy' | 'sweet'
    }
  ]
}
```

---

## 7. Theme System

```javascript
// Stored in localStorage
let currentTheme = localStorage.getItem('sp_theme') || 'dark'
let currentFont  = localStorage.getItem('sp_font')  || 'normal'
let reduceMotion = localStorage.getItem('sp_motion') === 'true'

function applyTheme(theme)
// Sets CSS custom properties on document.documentElement
// Available themes: 'dark' | 'light' | 'colorblind'

function applyFont(size)
// 'small' → 13px | 'normal' → 15px | 'large' → 18px
// Sets document.documentElement.style.fontSize

function applyMotion(reduce)
// true → injects <style id="no-motion-style"> with animation:none
// false → removes that style tag
```

CSS variables used throughout:
```css
--bg          /* Page background */
--panel       /* Card/panel background */
--ink         /* Primary text */
--orange      /* Primary accent (#FF6A2B in dark) */
--orange-deep /* Secondary accent */
--green       /* Success / paid / calories OK */
--red         /* Error / unpaid */
--muted       /* Secondary text */
--line        /* Border colour */
```

---

## 8. Settings & Admin Panel

Access: **Profile pill (top right) → ⚙️ Settings**

```javascript
window.showSettings = function()
// Renders modal with:
// - Theme selector (Dark / Light / Colour Blind)
// - Font size (Small / Normal / Large)
// - Reduce motion toggle
// - Admin tools section

window.cleanupAccounts = async function()
// Deletes root-level 'orders' collection (legacy)
// Deletes all user accounts EXCEPT jishu_at_gmail_com + anya_at_gmail_com
// Deletes all their subcollection orders first (Firestore doesn't cascade delete)
```

---

## 9. Restaurant Dashboard — `restaurant-dashboard.html`

### Login
Select restaurant from dropdown → **View Analytics →**

No password required — this is a role-based view (restaurant sees only their data).

### Data fetching
```javascript
async function fetchOrders()
// 1. fsList('users') → get all user documents
// 2. For each user: fsList('users/{uid}/orders') → get their orders
// 3. Filter: order.restaurants.includes(currentResto) OR cart has items from this resto
// 4. Extract myItems (items from this restaurant only) and myRevenue
// Returns: filtered orders with myItems and myRevenue attached
```

### Charts rendered
- Top items bar chart (horizontal, by order count)
- Revenue by item (CSS SVG donut)
- Order status breakdown (CSS SVG donut)
- Orders over time (vertical bar timeline)

---

## 10. Business Analytics — `splitplate-analytics.html`

### Login
Password: `splitplate2025`

### Data fetching
```javascript
async function loadAll()
// 1. fsList('users') → all registered users
// 2. For each user → fsList('users/{uid}/orders') → all their orders
// 3. Aggregate: GMV, platform fees, restaurant counts, item counts, group sizes
// 4. Build per-restaurant revenue/order maps
// 5. Render all KPI cards + 6 charts + 2 tables
```

### KPIs computed
```javascript
totalGMV      = sum of all order grandTotal
totalItems    = sum of all order itemCount
platformFees  = totalOrders × 8   // ₹8 per order
avgOrderValue = totalGMV / totalOrders
activeRestos  = unique restaurant names across all orders
```

### fsList helper (shared by both dashboards)
```javascript
async function fsList(path)
// path: 'users' | 'users/uid/orders' | etc.
// Splits path by '/' and builds Firestore reference chain
// Returns array of {id, ...documentData}
```

---

## 11. Delivery Partner Assignment

```javascript
const DELIVERY_PARTNERS = [
  {name:'Ravi Kumar',  rating:'⭐ 4.8', bike:'KA01 AB 1234', avatar:'🧑'},
  {name:'Suresh Babu', rating:'⭐ 4.9', bike:'KA02 CD 5678', avatar:'👨'},
  {name:'Mahesh R.',   rating:'⭐ 4.7', bike:'KA03 EF 9012', avatar:'🧔'},
  {name:'Dinesh S.',   rating:'⭐ 4.8', bike:'KA04 GH 3456', avatar:'👦'},
  {name:'Kiran M.',    rating:'⭐ 4.9', bike:'KA05 IJ 7890', avatar:'🧑'},
];

// On confirmOrder():
const groups = groupCartByRestaurant(cart)
const shuffled = [...DELIVERY_PARTNERS].sort(() => Math.random() - 0.5)
groups.forEach((resto, i) => assign shuffled[i % shuffled.length])
// ETA = random(18, 33) minutes per restaurant
```

---

## 12. What Makes SplitPlate Different from Swiggy

| Feature | Swiggy | SplitPlate |
|---|---|---|
| Group ordering | ❌ | ✅ Tag items per person |
| Bill split per person | ❌ | ✅ Auto-calculated |
| Multi-restaurant single cart | ❌ | ✅ One checkout |
| Simultaneous delivery | ❌ | ✅ Multiple partners |
| Calorie tracking per person | Partial | ✅ Live bar per person |
| Mood-based menu filter | ❌ | ✅ Hungry/Lazy/Healthy/Sweet |
| AI support chatbot | ❌ | ✅ Claude-powered |
| Colour blind mode | ❌ | ✅ Deuteranopia palette |
| Business analytics dashboard | ❌ | ✅ Full BI dashboard |
| Restaurant analytics portal | ❌ | ✅ Per-restaurant view |

---

## 13. Known Limitations

1. **Authentication** — Plain text passwords in Firestore. Use Firebase Auth in production.
2. **File protocol** — Must open in Chrome via Ctrl+O for Firebase to load. VS Code Live Server may block the CDN.
3. **No offline support** — Requires internet for Firebase to sync.
4. **Single organiser** — One person (You) pays the full bill. Group payment splitting is done informally via QR codes.
5. **Simulated payments** — QR codes and payment confirmation are UI-only. No real payment gateway integrated.

---

## 14. Extending the Project

### Add a new restaurant
In `swiggy-split.html`, add an entry to the `RESTAURANTS` array:
```javascript
{
  id:'newresto', name:'New Restaurant', emoji:'🍜',
  cuisine:'Cuisine Type', rating:4.5, time:'20-30 min', badge:null,
  desc:'Short description',
  menu:[
    {name:'Dish Name', price:150, cal:400, mood:['hungry','lazy']},
  ]
}
```
Also add to the dropdown in `restaurant-dashboard.html`.

### Add a new theme
In `swiggy-split.html`, add to the `THEMES` object:
```javascript
THEMES.mytheme = {
  '--bg':    '#...',
  '--panel': 'rgba(...)',
  '--ink':   '#...',
  '--orange':'#...',
  // ... all CSS variables
}
```
Then add a button in `showSettings()`.

---

*SplitPlate · Christ (Deemed to be University) · B.Sc. Economics & Data Science · CIA Assessment 2025*
