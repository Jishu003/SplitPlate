# 🍽️ SplitPlate — Group Order, Sorted

> **One cart. Separate bills. Separate plates.**

SplitPlate fixes the biggest problem with group food ordering — figuring out who owes what. Order from multiple restaurants in one session, tag each item to a person, and get automatic bill splits with per-person payment tracking.

Built as a CIA III project for B.Sc. Economics & Data Science at Christ (Deemed to be University), Bangalore — 2025.

---

## 🚀 How to Run

Open any file directly in Chrome (`Ctrl+O`):

| File | Who uses it | Login |
|---|---|---|
| `frontend/swiggy-split.html` | Customers ordering food | Sign up with email |
| `frontend/restaurant-dashboard.html` | Restaurant owners | Select restaurant from dropdown |
| `frontend/splitplate-analytics.html` | SplitPlate business team | Password: `splitplate2025` |

> **Important:** Open files in **Google Chrome** via `Ctrl+O`. Firebase requires internet to sync.

---

## ✨ Features

### Customer App
- 🍽️ **15 restaurants** with mood-based menu filter (Hungry / Lazy / Healthy / Sweet)
- 👥 **Per-person item tagging** — each item assigned to a specific person
- ➕ **Quantity control** — `[ − | qty | + ]` pill box for every cart item
- 💸 **Auto bill split** — each person's share calculated with shared fees + tax
- 🚚 **Multi-restaurant checkout** — order from multiple restaurants in one cart
- 🛵 **Simultaneous delivery** — separate delivery partners dispatched at the same time
- 📲 **QR payment per person** — organiser sends payment request individually
- 🔥 **Live calorie counter** — tracks per-person calories while ordering
- 🤖 **AI support chatbot** — powered by Claude
- 🎨 **3 themes** — Dark, Light, Colour Blind (Deuteranopia-optimised)
- 🔐 **Firebase auth** — sign up/sign in with email + password

### Restaurant Dashboard
- 📊 Revenue and order analytics per restaurant
- 🍽️ Top items by order count with visual bar charts
- 💸 Revenue breakdown by item (SVG donut chart)
- 📋 Order status tracking
- 📈 Orders over time timeline
- 📦 Recent orders table with customer details

### Business Analytics
- 💰 Platform-wide GMV (Gross Merchandise Value)
- 👥 User registry with spend tracking
- 🏪 Active restaurant performance across all 15
- 💳 Platform fee collection summary
- 📈 Cross-platform revenue and order charts
- 📦 All orders table across all users

---

## 🗂️ Project Structure

```
splitplate/
├── README.md
├── firebase.json
├── .gitignore
├── splitplate-agent-prompt.md
│
├── frontend/
│   ├── swiggy-split.html
│   ├── restaurant-dashboard.html
│   └── splitplate-analytics.html
│
├── backend/
│   ├── firestore.rules
│   ├── firestore.indexes.json
│   └── README.md
│
└── docs/
    ├── architecture.md
    └── project-implementation.md
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript |
| Database | Firebase Firestore (NoSQL, serverless) |
| Auth | Custom Firestore email/password |
| Charts | Pure CSS + SVG |
| Fonts | Google Fonts (Inter, Playfair Display) |
| AI Chatbot | Anthropic Claude API |

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

| Name | Role | Responsibilities |
|---|---|---|
| **Upanshu** | Lead Developer & Architect | Firebase backend, authentication, bill-split algorithm, system architecture, deployment |
| **Aanya** | Frontend Developer | UI/UX design, menu system, cart screen, dark theme, CSS animations |
| **Vibhi** | Frontend Developer | Restaurant list, offers banner, calorie tracker, settings & themes, accessibility |
| **Agastya** | Backend & Analytics | Database schema, analytics dashboards, Firestore queries, data aggregation |
| **Akankshya** | Full Stack & QA | Restaurant dashboard, profile modal, security, documentation, testing |

**Course:** B.Sc. Economics & Data Science
**Institution:** Christ (Deemed to be University), Bangalore
**Assessment:** CIA III — Digital Business Systems ECD223-3 (2025)

---

