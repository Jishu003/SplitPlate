# SplitPlate — Project Implementation Tracker
**CIA III | Digital Business Systems | ECD223-3**
Christ (Deemed to be University) | B.Sc. Economics & Data Science 2025

---

## Team Members & Roles

| Name | Role | Core Responsibilities |
|---|---|---|
| **Upanshu** | Lead Developer & System Architect | Firebase backend, authentication, bill-split algorithm, deployment, architecture |
| **Aanya** | Frontend Developer — UI/UX | Login screen, menu, cart UI, CSS animations, dark theme |
| **Vibhi** | Frontend Developer — Components | Restaurant list, offers banner, calorie tracker, settings, accessibility |
| **Agastya** | Backend & Analytics Developer | Database schema, Firestore queries, analytics dashboards, data aggregation |
| **Akankshya** | Full Stack Developer & QA | Restaurant dashboard, profile, QR payment, security, documentation, testing |

---

## Business Algorithm — Bill Split Engine

### Problem Being Solved
In a group food order, different people order different items. A fair split must charge each person only for what they ordered, plus a proportional share of delivery and platform fees, plus tax on their own items. People who did not order anything should pay Rs 0.

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

### Example
```
Upanshu: Rs460 items + Rs24 fees + Rs23 tax = Rs507
Aanya:   Rs30  items + Rs24 fees + Rs1.5 tax = Rs55.50
Vibhi:   Rs0 items → Rs0 total (no fees charged)
```

---

## Task Implementation Log

| Task ID | Task | Component | Assigned To | Status | Date | AI Assistance | Evidence |
|---|---|---|---|---|---|---|---|
| T001 | Project setup — single HTML architecture decision | Architecture | **Upanshu** | Completed | Aug 2025 | Yes | swiggy-split.html |
| T002 | Firebase project setup (splitplat) | Backend | **Upanshu** | Completed | Aug 2025 | No | Firebase console |
| T003 | Firestore security rules — open rules for development | Backend | **Upanshu** | Completed | Aug 2025 | No | backend/firestore.rules |
| T004 | Customer login screen — dark glassmorphism UI | Frontend | **Aanya** | Completed | Aug 2025 | Yes | Login section HTML/CSS |
| T005 | Sign Up flow — Firestore custom authentication | Authentication | **Upanshu** | Completed | Aug 2025 | Yes | doSignUp() |
| T006 | Sign In — cross-verification with Firestore | Authentication | **Upanshu** | Completed | Aug 2025 | Yes | doSignIn() |
| T007 | Email key convention — safe Firestore document IDs | Backend | **Upanshu** | Completed | Aug 2025 | No | emailKey() |
| T008 | RESTAURANTS array — 6 restaurants with menus and mood tags | Data | **Aanya** | Completed | Aug 2025 | Yes | RESTAURANTS const |
| T009 | Expand to 15 restaurants — Corner House, Taco Bell, Uru, Norwa Chai, Empire, Belgian Waffles, Howlers, Smash Guys, Rameshwaram | Data | **Vibhi** | Completed | Aug 2025 | Yes | RESTAURANTS array |
| T010 | Offers banner — 5 animated slides, auto-rotate, swipe | Frontend | **Vibhi** | Completed | Aug 2025 | Yes | initOffersBanner() |
| T011 | Restaurant list — scrollable cards with rating, time, badge | Frontend | **Vibhi** | Completed | Aug 2025 | Yes | renderRestoCards() |
| T012 | Offer CTA buttons — navigate to correct restaurant | Frontend | **Vibhi** | Completed | Aug 2025 | Yes | offerAction() |
| T013 | Menu screen — items list with mood filter | Frontend | **Aanya** | Completed | Aug 2025 | Yes | renderMenu() |
| T014 | Per-person item tagging — person dropdown + Add | Frontend | **Aanya** | Completed | Aug 2025 | Yes | addMenuItem() |
| T015 | Quantity control — pill box with qty scaling | Frontend | **Aanya** | Completed | Aug 2025 | Yes | changeQty(), qty-pill |
| T016 | **BILL SPLIT ALGORITHM — personAmount()** | Algorithm | **Upanshu** | Completed | Aug 2025 | Yes | personAmount() |
| T017 | recalcTotals() — live bill update every cart change | Algorithm | **Upanshu** | Completed | Aug 2025 | Yes | recalcTotals() |
| T018 | Cart screen — order summary strip and invoice | Frontend | **Aanya** | Completed | Aug 2025 | Yes | renderBill() |
| T019 | Per-person bill cards with exact split amounts | Frontend | **Aanya** | Completed | Aug 2025 | Yes | bill-person-block |
| T020 | Live calorie counter per person in menu screen | Frontend | **Vibhi** | Completed | Aug 2025 | Yes | updateMenuCalStrip() |
| T021 | Pay Full Bill — Are you sure? confirmation modal | Frontend | **Akankshya** | Completed | Aug 2025 | Yes | payFullBillNow() |
| T022 | Buffer/review panel — item review after payment | Frontend | **Akankshya** | Completed | Aug 2025 | Yes | renderBufferSection() |
| T023 | **MULTI-DELIVERY ALGORITHM — assign partners per restaurant** | Algorithm | **Upanshu** | Completed | Aug 2025 | Yes | confirmOrder() |
| T024 | QR payment flow per person | Frontend | **Akankshya** | Completed | Aug 2025 | Yes | sendQR() |
| T025 | Refund system — refund per removed item | Frontend | **Akankshya** | Completed | Aug 2025 | Yes | bufferRefund() |
| T026 | Firebase saveToFirebase() — debounced 1000ms | Backend | **Upanshu** | Completed | Aug 2025 | Yes | saveToFirebase() |
| T027 | perPersonDetails — per-person breakdown in Firestore | Backend | **Agastya** | Completed | Aug 2025 | Yes | perPerson{} object |
| T028 | Past orders modal — read from Firebase subcollection | Backend | **Agastya** | Completed | Aug 2025 | Yes | showPastOrders() |
| T029 | Start New Order — full state reset, new orderId | Frontend | **Upanshu** | Completed | Aug 2025 | Yes | startNewOrder() |
| T030 | User pill — show logged-in name on all 3 screens | Frontend | **Aanya** | Completed | Aug 2025 | Yes | renderUserPill() |
| T031 | Profile modal — stats, people, past orders link | Frontend | **Akankshya** | Completed | Aug 2025 | Yes | showProfile() |
| T032 | Settings modal — Dark/Light/Colour Blind themes | Frontend | **Vibhi** | Completed | Aug 2025 | Yes | showSettings() |
| T033 | Colour Blind mode — Deuteranopia blue/amber palette | Accessibility | **Vibhi** | Completed | Aug 2025 | Yes | THEMES.colorblind |
| T034 | Font size toggle — Small / Normal / Large | Accessibility | **Vibhi** | Completed | Aug 2025 | Yes | applyFont() |
| T035 | Reduce animations toggle | Accessibility | **Vibhi** | Completed | Aug 2025 | Yes | applyMotion() |
| T036 | AI support chatbot — Claude API integration | Integration | **Agastya** | Completed | Aug 2025 | Yes | Chatbot section |
| T037 | Admin cleanup — delete accounts from Firebase | Backend | **Upanshu** | Completed | Aug 2025 | Yes | cleanupAccounts() |
| T038 | Auto-cleanup — delete legacy root orders on login | Backend | **Upanshu** | Completed | Aug 2025 | Yes | goHome() |
| T039 | Add person by email — fetch name from Firebase | Feature | **Agastya** | Completed | Aug 2025 | Yes | fetchPersonByEmail() |
| T040 | Restaurant Dashboard — login and layout | Frontend | **Akankshya** | Completed | Aug 2025 | Yes | restaurant-dashboard.html |
| T041 | Restaurant Dashboard — fetchOrders() from Firebase | Backend | **Agastya** | Completed | Aug 2025 | Yes | fetchOrders() |
| T042 | Restaurant Dashboard — 4 charts | Frontend | **Akankshya** | Completed | Aug 2025 | Yes | Chart functions |
| T043 | SplitPlate Analytics — admin login + 8 KPI cards | Frontend | **Aanya** | Completed | Aug 2025 | Yes | splitplate-analytics.html |
| T044 | Analytics — loadAll() aggregates all orders | Backend | **Agastya** | Completed | Aug 2025 | Yes | loadAll() |
| T045 | Analytics — 6 charts across platform | Frontend | **Agastya** | Completed | Aug 2025 | Yes | Chart renderers |
| T046 | GitHub repo structure — frontend/ backend/ docs/ | DevOps | **Upanshu** | Completed | Aug 2025 | No | GitHub repo |
| T047 | Firestore rules file | Security | **Upanshu** | Completed | Aug 2025 | No | firestore.rules |
| T048 | Firebase.json — hosting config | DevOps | **Agastya** | Completed | Aug 2025 | Yes | firebase.json |
| T049 | SplitPlate DevAgent prompt | AI/DevOps | **Upanshu** | Completed | Aug 2025 | Yes | agent-prompt.md |
| T050 | Architecture document — CIA III | Documentation | **Upanshu** | Completed | Aug 2025 | Yes | docs/architecture.md |
| T051 | Project implementation tracker — CIA III | Documentation | **Akankshya** | Completed | Aug 2025 | Yes | This file |
| T052 | Input validation — email, password, empty checks | Security | **Akankshya** | Completed | Aug 2025 | Yes | Sign in/up validation |
| T053 | Error handling — try/catch all Firebase calls | Security | **Upanshu** | Completed | Aug 2025 | Yes | All async calls |
| T054 | Dark theme — CSS variables, glassmorphism | Frontend | **Aanya** | Completed | Aug 2025 | Yes | :root CSS |
| T055 | Scalability analysis — quantitative calculations | Documentation | **Agastya** | Completed | Aug 2025 | Yes | architecture.md S13 |
| T056 | Security mechanisms documentation | Documentation | **Akankshya** | Completed | Aug 2025 | Yes | architecture.md S14 |
| T057 | Failure and recovery analysis | Documentation | **Akankshya** | Completed | Aug 2025 | Yes | architecture.md S15 |

---

## Task Distribution Summary

| Member | Tasks | Count |
|---|---|---|
| **Upanshu** | T001–T003, T005–T007, T016–T017, T023, T026, T029, T037–T038, T046–T047, T049–T050, T053 | 15 tasks |
| **Aanya** | T004, T008, T013–T015, T018–T019, T030, T043, T054 | 10 tasks |
| **Vibhi** | T009–T012, T020, T032–T035 | 9 tasks |
| **Agastya** | T027–T028, T036, T039, T041, T044–T045, T048, T055 | 9 tasks |
| **Akankshya** | T021–T022, T024–T025, T031, T040, T042, T051–T052, T056–T057 | 10 tasks |

---

## CRUD Operations

| Operation | Function | Firebase Call |
|---|---|---|
| **Create** User | doSignUp() | db.collection('users').doc(uid).set({}) |
| **Create** Order | saveToFirebase() | db.collection('users').doc(uid).collection('orders').doc(id).set({}) |
| **Read** User | doSignIn() | db.collection('users').doc(uid).get() |
| **Read** Orders | showPastOrders(), dashboards | db.collection('users').doc(uid).collection('orders').get() |
| **Update** lastLogin | doSignIn() | db.collection('users').doc(uid).update({lastLogin}) |
| **Update** orderState | saveFinalOrder() | .update({orderState:'sent'}) |
| **Delete** Legacy orders | cleanupAccounts() | doc.ref.delete() |

---

*Last updated: August 2025*
*Team: Upanshu · Aanya · Vibhi · Agastya · Akankshya*
*Christ (Deemed to be University) | ECD223-3 Digital Business Systems*
