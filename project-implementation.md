# SplitPlate — Project Implementation Tracker
**CIA III | Digital Business Systems | ECD223-3**
Christ (Deemed to be University) | B.Sc. Economics & Data Science 2025

> This document is the single work log and contribution record for the SplitPlate project.
> Updated throughout development as tasks are planned, started, completed, or changed.

---

## Team

| Student | Role |
|---|---|
| Jishu | Lead Developer — Firebase, bill-split algorithm, analytics, architecture |
| Anya | Frontend Developer — UI/UX, menu system, cart, testing |

---

## Business Algorithm — Bill Split Engine

### Problem Being Solved
In a group food order, different people order different items. A fair split must charge each person only for what they ordered, plus a proportional share of delivery and platform fees, plus tax on their own items. People who did not order anything should pay ₹0.

### Input
- `cart[]` — array of items, each tagged to a person
- `people[]` — list of names in the group
- `DELIVERY_FEE = ₹40`, `PLATFORM_FEE = ₹8`, `TAX_RATE = 5%`

### Processing Logic
```
For each person P in people[]:
  1. Filter cart items where item.person === P
  2. itemTotal(P)    = sum of item.price for P's items
  3. If itemTotal(P) == 0 → totalOwed(P) = 0 (skip fees)
  4. orderingCount   = number of people with at least 1 item
  5. sharedFees(P)   = (DELIVERY_FEE + PLATFORM_FEE) / orderingCount
  6. tax(P)          = itemTotal(P) × TAX_RATE
  7. totalOwed(P)    = itemTotal(P) + sharedFees(P) + tax(P)
```

### Pseudocode
```
function personAmount(person):
  items = cart.filter(c => c.person == person)
  itemTotal = sum(items.price)
  if itemTotal == 0: return 0
  orderingCount = people.filter(p => cart.any(c => c.person == p)).length
  sharedFees = (40 + 8) / orderingCount
  tax = itemTotal * 0.05
  return itemTotal + sharedFees + tax
```

### Where Implemented
File: `frontend/swiggy-split.html`
Function: `personAmount(person)` — approximately line 2584
Also called in: `recalcTotals()`, `renderBill()`, `saveToFirebase()`

### Example Input
```
cart = [
  {item: "Chicken Biryani", price: 240, person: "You"},
  {item: "Filter Coffee",   price: 30,  person: "Anya"},
  {item: "Death by Choc",   price: 220, person: "You"}
]
people = ["You", "Anya", "Jishu"]   // Jishu has no items
```

### Example Output
```
You:   (240 + 220) + (48/2) + (460 × 0.05) = 460 + 24 + 23  = ₹507
Anya:  30          + (48/2) + (30  × 0.05) = 30  + 24 + 1.5 = ₹55.50
Jishu: 0 items → ₹0 (no fees charged)
Grand Total: ₹507 + ₹55.50 = ₹562.50
```

---

## Task Implementation Log

| Task ID | Task | Component | Assigned To | Status | Completed By | Date Completed | AI Assistance | Evidence |
|---|---|---|---|---|---|---|---|---|
| T001 | Project setup — single HTML file architecture decision | Architecture | Jishu | Completed | Jishu | Aug 2025 | Yes | swiggy-split.html created |
| T002 | Firebase project setup (splitplat) | Backend | Jishu | Completed | Jishu | Aug 2025 | No | Firebase console |
| T003 | Firestore security rules — initial open rules for development | Backend | Jishu | Completed | Jishu | Aug 2025 | No | backend/firestore.rules |
| T004 | Customer login screen — dark glassmorphism UI with tabs | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | swiggy-split.html login section |
| T005 | Custom Firestore authentication — Sign Up flow | Authentication | Jishu | Completed | Jishu | Aug 2025 | Yes | doSignUp() function |
| T006 | Custom Firestore authentication — Sign In with cross-verification | Authentication | Jishu | Completed | Jishu | Aug 2025 | Yes | doSignIn() function |
| T007 | Email key convention — safe Firestore document IDs | Backend | Jishu | Completed | Jishu | Aug 2025 | No | emailKey() function |
| T008 | RESTAURANTS array — 6 restaurants with menu and mood tags | Data | Anya | Completed | Anya | Aug 2025 | Yes | RESTAURANTS const in swiggy-split.html |
| T009 | Expand RESTAURANTS to 15 — add Corner House, Taco Bell, Uru, Norwa Chai, Empire, Belgian Waffles, Howlers, Smash Guys, Rameshwaram | Data | Anya | Completed | Anya | Aug 2025 | Yes | RESTAURANTS array extended |
| T010 | Home screen — animated offers banner (5 slides, auto-rotate, swipe) | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | initOffersBanner(), offer slides |
| T011 | Restaurant list — vertical scrollable cards with rating, time, badge | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | renderRestoCards() |
| T012 | Offer banner CTA buttons — navigate to correct restaurant on click | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | offerAction() |
| T013 | Menu screen — items list with mood filter | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | renderMenu(), mood filter buttons |
| T014 | Per-person item tagging — dropdown + Add button | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | personSelect, addMenuItem() |
| T015 | Quantity control — `[ − | qty | + ]` pill box with qty scaling | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | changeQty(), qty-pill CSS |
| T016 | **BILL SPLIT ALGORITHM — personAmount()** | Algorithm | Jishu | Completed | Jishu | Aug 2025 | Yes | personAmount() function |
| T017 | recalcTotals() — live total update after every cart change | Algorithm | Jishu | Completed | Jishu | Aug 2025 | Yes | recalcTotals() |
| T018 | Cart screen — order summary strip (total, collected, pending) | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | renderBill() |
| T019 | Per-person bill cards in bill section | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | bill-person-block rendering |
| T020 | Live calorie counter per person in menu screen | Frontend | Jishu | Completed | Jishu | Aug 2025 | Yes | updateMenuCalStrip() |
| T021 | Pay Full Bill — "Are you sure?" confirmation modal | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | payFullBillNow(), confirmPayOverlay |
| T022 | Buffer/review panel — show items after payment, before confirm | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | renderBufferSection() |
| T023 | **MULTI-DELIVERY ALGORITHM — assign partners per restaurant** | Algorithm | Jishu | Completed | Jishu | Aug 2025 | Yes | confirmOrder(), DELIVERY_PARTNERS |
| T024 | QR payment flow per person | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | sendQR(), pay-btn |
| T025 | Refund system — refund per removed item | Frontend | Jishu | Completed | Jishu | Aug 2025 | Yes | bufferRefund() |
| T026 | Firebase saveToFirebase() — debounced order save | Backend | Jishu | Completed | Jishu | Aug 2025 | Yes | saveToFirebase() — 1000ms debounce |
| T027 | perPersonDetails — per-person breakdown saved to Firestore | Backend | Jishu | Completed | Jishu | Aug 2025 | Yes | perPerson{} in saveToFirebase() |
| T028 | Past orders modal — read from Firebase subcollection | Backend | Jishu | Completed | Jishu | Aug 2025 | Yes | showPastOrders(), showOrdersModal() |
| T029 | Start New Order — reset all state, new orderId | Frontend | Jishu | Completed | Jishu | Aug 2025 | Yes | startNewOrder() |
| T030 | User pill — show logged-in name on all 3 screens | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | renderUserPill() |
| T031 | Profile modal — user stats, people in order, past orders | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | showProfile() |
| T032 | Settings modal — Dark/Light/Colour Blind themes | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | showSettings(), applyTheme() |
| T033 | Colour Blind mode — Deuteranopia-optimised blue/amber palette | Accessibility | Anya | Completed | Anya | Aug 2025 | Yes | THEMES.colorblind, [data-cb] CSS |
| T034 | Font size toggle (Small/Normal/Large) | Accessibility | Anya | Completed | Anya | Aug 2025 | Yes | applyFont() |
| T035 | Reduce animations toggle | Accessibility | Anya | Completed | Anya | Aug 2025 | Yes | applyMotion() |
| T036 | AI support chatbot — Claude API integration | Integration | Jishu | Completed | Jishu | Aug 2025 | Yes | AI chatbot section in cart screen |
| T037 | Admin cleanup — delete all accounts except specified ones | Backend | Jishu | Completed | Jishu | Aug 2025 | Yes | cleanupAccounts() |
| T038 | Auto-cleanup — delete legacy root orders collection on login | Backend | Jishu | Completed | Jishu | Aug 2025 | Yes | goHome() auto-cleanup |
| T039 | Add person by email — fetch name from Firebase users | Feature | Jishu | Completed | Jishu | Aug 2025 | Yes | fetchPersonByEmail(), addPersonBtn |
| T040 | Restaurant Dashboard — login dropdown, analytics layout | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | restaurant-dashboard.html |
| T041 | Restaurant Dashboard — fetchOrders() reads from Firebase | Backend | Jishu | Completed | Jishu | Aug 2025 | Yes | fetchOrders() in restaurant-dashboard |
| T042 | Restaurant Dashboard — 4 charts (bar, donut × 2, timeline) | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | renderBarChart(), renderDonut(), renderTimeline() |
| T043 | SplitPlate Analytics Dashboard — admin login + 8 KPI cards | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | splitplate-analytics.html |
| T044 | Analytics — loadAll() aggregates all users + all orders | Backend | Jishu | Completed | Jishu | Aug 2025 | Yes | loadAll() in splitplate-analytics |
| T045 | Analytics — 6 charts: revenue by resto, orders by resto, timeline, status, top items, group size | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | renderBarChart() × 4, renderDonut() × 2 |
| T046 | GitHub repo structure — frontend/, backend/, docs/ | DevOps | Jishu | Completed | Jishu | Aug 2025 | No | splitplate-github.zip |
| T047 | Firestore rules file — backend/firestore.rules | Security | Jishu | Completed | Jishu | Aug 2025 | No | backend/firestore.rules |
| T048 | Firebase.json — hosting config with rewrites | DevOps | Jishu | Completed | Jishu | Aug 2025 | Yes | firebase.json |
| T049 | SplitPlate DevAgent prompt — autonomous developer agent | AI/DevOps | Jishu | Completed | Jishu | Aug 2025 | Yes | splitplate-agent-prompt.md |
| T050 | Architecture document — CIA III requirement | Documentation | Jishu | Completed | Jishu | Aug 2025 | Yes | docs/architecture.md |
| T051 | Project implementation tracker — CIA III requirement | Documentation | Both | Completed | Both | Aug 2025 | Yes | docs/project-implementation.md |
| T052 | Input validation — email regex, password length, empty field checks | Security | Jishu | Completed | Jishu | Aug 2025 | Yes | doSignIn(), doSignUp() validation |
| T053 | Error handling — try/catch on all Firebase operations | Security | Jishu | Completed | Jishu | Aug 2025 | Yes | All async Firebase calls |
| T054 | Dark theme — CSS custom properties, glass morphism panels | Frontend | Anya | Completed | Anya | Aug 2025 | Yes | :root CSS variables, body::before gradient |
| T055 | Scalability analysis — quantitative calculations | Documentation | Jishu | Completed | Jishu | Aug 2025 | Yes | docs/architecture.md §13 |
| T056 | Security mechanisms documentation — 10 mechanisms | Documentation | Jishu | Completed | Jishu | Aug 2025 | Yes | docs/architecture.md §14 |
| T057 | Failure and recovery analysis — 5 scenarios | Documentation | Jishu | Completed | Jishu | Aug 2025 | Yes | docs/architecture.md §15 |

---

## Business Algorithms Summary

### Algorithm 1 — Per-Person Bill Split (T016)
**Implemented in:** `personAmount(person)` — `frontend/swiggy-split.html`
**What it does:** Calculates each person's exact share: their item total + proportional share of fees (only if they ordered) + 5% tax on their own items.

### Algorithm 2 — Multi-Restaurant Delivery Allocation (T023)
**Implemented in:** `confirmOrder()` — `frontend/swiggy-split.html`
**What it does:** Groups cart items by restaurant, assigns a randomly shuffled delivery partner to each restaurant, generates a unique ETA per delivery, and displays all deliveries simultaneously.

### Algorithm 3 — Duplicate Item Merging (T015)
**Implemented in:** `addMenuItem()` — `frontend/swiggy-split.html`
**What it does:** When the same item is added for the same person again, instead of creating a duplicate cart entry it increments `qty` and recalculates `price = basePrice × qty`.

### Algorithm 4 — Debounced Firebase Write (T026)
**Implemented in:** `saveToFirebase()` — `frontend/swiggy-split.html`
**What it does:** Uses a 1000ms debounce timer — every cart change restarts the timer, and only when 1 second passes with no further changes does the write execute. Prevents quota exhaustion.

---

## System Integration Evidence

The system demonstrates a complete loop:
```
User Interface → Application Logic → Data Layer → Business Output

Example:
1. User clicks "Add" on Chicken Biryani for "Riya" [UI]
2. addMenuItem() checks for duplicate, merges or adds [App Logic]
3. recalcTotals() runs personAmount() for all people [Business Logic]
4. saveToFirebase() writes to Firestore after 1000ms [Data Layer]
5. Bill section shows Riya owes ₹268, You owes ₹507 [Business Output]
6. Restaurant Dashboard reads this order in real time [BI Output]
```

---

## CRUD Operations

| Operation | Where | Firebase Call |
|---|---|---|
| **Create** User | doSignUp() | `db.collection('users').doc(uid).set({...})` |
| **Create** Order | saveToFirebase() | `db.collection('users').doc(uid).collection('orders').doc(orderId).set({...})` |
| **Read** User | doSignIn() | `db.collection('users').doc(uid).get()` |
| **Read** Orders | showPastOrders(), dashboards | `db.collection('users').doc(uid).collection('orders').get()` |
| **Update** lastLogin | doSignIn() | `db.collection('users').doc(uid).update({lastLogin})` |
| **Update** orderState | saveFinalOrder() | `db.collection('users').doc(uid).collection('orders').doc(id).update({orderState:'sent'})` |
| **Delete** Legacy orders | cleanupAccounts(), goHome() | `doc.ref.delete()` |

---

*Last updated: August 2025 | SplitPlate CIA III Submission*
