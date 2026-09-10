# 🍽️ SplitPlate — Group Order, Sorted

> **One cart. Separate bills. Separate plates.**

SplitPlate fixes the biggest problem with group food ordering — figuring out who owes what. Order from multiple restaurants in one session, tag each item to a person, and get automatic bill splits with per-person payment tracking.

Built as a CIA (Continuous Internal Assessment) project for B.Sc. Economics & Data Science at Christ (Deemed to be University), Bangalore.

---

## 🚀 Live Demo

Open any file directly in Chrome (`Ctrl+O`):

| File | Who uses it | Login |
|---|---|---|
| `frontend/swiggy-split.html` | Customers ordering food | Sign up with email |
| `frontend/restaurant-dashboard.html` | Restaurant owners | Select restaurant from dropdown |
| `frontend/splitplate-analytics.html` | SplitPlate business team | Password: `splitplate2025` |

> **Important:** Open files in **Google Chrome** via `Ctrl+O`. Firebase requires internet to sync.

---

## ✨ Features

### Customer App (`swiggy-split.html`)
- 🍽️ **15 restaurants** with mood-based menu filter (Hungry / Lazy / Healthy / Sweet)
- 👥 **Per-person item tagging** — each item assigned to a specific person
- ➕ **Quantity control** — `[ − | 2 | + ]` pill box for every cart item
- 💸 **Auto bill split** — each person's share calculated with shared fees + tax
- 🚚 **Multi-restaurant checkout** — order from Corner House AND Meghna Foods in one cart
- 🛵 **Simultaneous delivery** — separate delivery partners dispatched at the same time
- 📲 **QR payment per person** — organiser sends payment request to each person
- 🔥 **Live calorie counter** — tracks per-person calories while ordering
- 🤖 **AI support chatbot** — powered by Claude
- 🎨 **3 themes** — Dark, Light, Colour Blind (Deuteranopia-optimised)
- 🔐 **Firebase auth** — sign up/sign in with email + password

### Restaurant Dashboard (`restaurant-dashboard.html`)
- 📊 Revenue and order analytics per restaurant
- 🍽️ Top items by order count
- 💸 Revenue breakdown by item
- 📋 Order status tracking
- 🕐 Orders over time timeline
- 📦 Recent orders table with customer details

### Business Analytics (`splitplate-analytics.html`)
- 💰 Platform-wide GMV (Gross Merchandise Value)
- 👥 User registry with spend tracking
- 🏪 Active restaurant performance
- 💳 Platform fee collection summary
- 📈 Cross-platform revenue and order charts
- 📦 All orders table across all users

---

## 🗂️ Project Structure

```
splitplate/
├── README.md                          ← You are here
├── firebase.json                      ← Firebase hosting config
│
├── frontend/                          ← All UI files
│   ├── swiggy-split.html             ← Customer ordering app
│   ├── restaurant-dashboard.html     ← Restaurant analytics
│   └── splitplate-analytics.html    ← Business BI dashboard
│
├── backend/                           ← Firebase configuration
│   ├── firestore.rules               ← Firestore security rules
│   ├── firestore.indexes.json        ← Composite index definitions
│   └── README.md                     ← Backend setup guide
│
└── docs/                              ← Documentation
    ├── ARCHITECTURE.md               ← System design & data flow
    └── IMPLEMENTATION.md             ← Code guide & function reference
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript (no framework) |
| Database | Firebase Firestore (NoSQL, serverless) |
| Auth | Custom Firestore-based email/password |
| Charts | Pure CSS + SVG (no chart library) |
| Fonts | Google Fonts (Inter, Playfair Display) |
| AI chatbot | Anthropic Claude API |

---

## ⚡ Quick Start

### 1. Clone the repo
```bash
git clone https://github.com/your-username/splitplate.git
cd splitplate
```

### 2. Set up Firebase rules
Go to [Firebase Console](https://console.firebase.google.com/project/splitplat) → Firestore Database → Rules → paste `backend/firestore.rules` → Publish.

### 3. Open in Chrome
```
Chrome → Ctrl+O → frontend/swiggy-split.html
```

### 4. Sign up and start ordering!

---

## 🗃️ Database Structure

```
Firestore
└── users/{emailKey}/
    ├── name, email, password, createdAt, lastLogin
    └── orders/{orderId}/
        ├── grandTotal, restaurants, orderState
        ├── cart [{item, price, person, resto, qty}]
        ├── people ["You", "Riya", "Karan"]
        └── perPersonDetails {
              "Riya": { totalOwed, items, tax, paymentStatus }
            }
```

---

## 📊 What Makes SplitPlate Different from Swiggy

| Feature | Swiggy | SplitPlate |
|---|---|---|
| Group ordering with bill split | ❌ | ✅ |
| Multi-restaurant single cart | ❌ | ✅ |
| Simultaneous delivery partners | ❌ | ✅ |
| Per-person calorie tracking | ❌ | ✅ |
| Mood-based menu filter | ❌ | ✅ |
| AI support chatbot | ❌ | ✅ |
| Colour blind accessibility mode | ❌ | ✅ |
| Restaurant analytics portal | ❌ | ✅ |
| Business BI dashboard | ❌ | ✅ |

---

## 👨‍💻 Team

| Name | Role |
|---|---|
| Jishu | Lead Developer, Firebase, Analytics |
| Anya | Frontend, UX, Testing |

**Course:** B.Sc. Economics & Data Science
**Institution:** Christ (Deemed to be University), Bangalore
**Assessment:** CIA — Microeconomics / Data Science Project (2025)

---

## 📄 License

MIT License — free to use, modify, and distribute.
