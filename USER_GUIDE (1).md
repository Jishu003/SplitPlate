# 📖 SplitPlate — Complete User Guide

> **One cart. Separate bills. Separate plates.**
> This guide covers all 3 SplitPlate applications.

---

## 🚀 Getting Started — All 3 Apps

| App | File | Who Uses It | Login |
|---|---|---|---|
| 🍽️ **Customer App** | `swiggy-split.html` | Anyone ordering food | Sign up with email |
| 📊 **Restaurant Dashboard** | `restaurant-dashboard.html` | Restaurant owners | Select restaurant from dropdown |
| 📈 **Business Analytics** | `splitplate-analytics.html` | SplitPlate admin only | Password: `SplitPlate@Admin2025!` |

### How to Open Any File
1. Open **Google Chrome**
2. Press `Ctrl + O`
3. Select the HTML file
4. Done!

> ⚠️ Always use **Google Chrome**. Other browsers may not load Firebase correctly.

---

---

# 🍽️ APP 1 — Customer App (`swiggy-split.html`)

---

## 👤 Account Setup

### Sign Up (New User)
1. Click the **Sign Up** tab
2. Enter your **full name**
3. Enter your **email address**
4. Create a **password** (minimum 6 characters)
5. Click **Create Account →**
6. You're in! 🎉

### Sign In (Returning User)
1. Click the **Sign In** tab
2. Enter your **email** and **password**
3. Click **Sign In →**

> 🔒 Passwords are encrypted using SHA-256 hashing before being stored in Firebase. Nobody can see your real password.

---

## 🏠 Home Screen

After signing in you'll see:
- **Offers banner** — 5 sliding offers, tap any to jump to that restaurant
- **15 restaurants** — scroll down to browse all options
- **Your profile pill** — top right, shows your name

### Choosing a Restaurant
- Each card shows: rating ⭐, delivery time 🕐, cuisine type
- Tap any restaurant card to open its menu

---

## 🍽️ Menu Screen

### Step 1 — Add People to Your Order
1. Type a person's name in the **"Add person"** box
2. Press **+ Add**
3. Or enter their email — if they have a SplitPlate account their name is fetched automatically
4. Repeat for everyone (e.g. You, Riya, Karan)

### Step 2 — Filter by Mood
| Filter | What it shows |
|---|---|
| 🍔 All | Everything on the menu |
| 😋 Hungry | Hearty, filling meals |
| 😴 Lazy | Quick, easy options |
| 🥗 Healthy | Light, nutritious items |
| 🍰 Sweet | Desserts and treats |

### Step 3 — Add Items
1. Select **who this item is for** from the person dropdown
2. Click **Add** next to the item
3. Use `[ − | qty | + ]` pill to change quantity
4. Each item is tagged to the right person automatically

### Live Calorie Counter
A coloured bar appears for each person:
- 🟢 **Green** — under 60% of 2000 kcal daily budget
- 🟡 **Yellow** — approaching the limit
- 🔴 **Red** — over 2000 kcal

### Ordering from Multiple Restaurants
1. Go back to Home (tap 🏠)
2. Pick a second restaurant
3. Add more items — they all merge into one cart

---

## 🛒 Cart & Bill Screen

### How the Bill Split Works
```
Your amount = Your items + (₹48 ÷ people who ordered) + (Your items × 5% tax)
```

> ✅ People with NO items pay ₹0 — no fees charged to them!

**Example with 3 people (Vibhi orders nothing):**
```
Upanshu: ₹460 items + ₹24 fees + ₹23 tax = ₹507
Aanya:   ₹30  items + ₹24 fees + ₹1.5 tax = ₹55.50
Vibhi:   ₹0   items → ₹0 total
```

### Paying the Bill

**Step 1 — Organiser pays full amount:**
1. Tap **"Pay Full Bill"**
2. Confirm the grand total in the popup
3. Tap **"Yes, I've paid ₹XXX"**

**Step 2 — Collect from each person:**
1. Each person gets a payment card
2. Tap **"Send QR"** to share a QR code with them
3. Once they pay tap **"Mark as Paid"** ✅
4. Cards turn green when settled

---

## 🚚 Placing the Order

### Review & Confirm
1. A **buffer panel** appears — review all items one last time
2. Remove any wrong items here (refunds calculated automatically)
3. Tap **"Place Order / Confirm"**

### Delivery Partners
- One delivery partner assigned **per restaurant**
- 2 restaurants ordered = 2 simultaneous deliveries
- Each card shows: name, rating, bike number, estimated time

---

## 💸 Refunds

If an item is removed after payment:
1. Tap **"Refund / Settle"** on the person's card
2. A modal shows refundable items — tick the ones to refund
3. Select a reason (Wrong item / Changed mind / Unavailable)
4. Tap **"Process Refund"**
5. A receipt is generated automatically

---

## 🤖 AI Support Chatbot

1. Scroll to the bottom of the cart screen
2. Tap **"Ask SplitPlate AI"**
3. Type your question
4. Get an instant answer powered by Claude AI

**Example questions:**
- *"How do I split the bill?"*
- *"My item was wrong, what do I do?"*
- *"How do I add someone to my order?"*

---

## ⚙️ Settings

Access via **Profile pill (top right) → ⚙️ Settings**

### Themes
| Theme | Description |
|---|---|
| 🌑 Dark | Default — easy on the eyes |
| ☀️ Light | Bright mode |
| 👁️ Colour Blind | Deuteranopia-friendly blue/amber palette |

### Text Size
**Small** / **Normal** / **Large** — changes the whole app font size

### Reduce Animations
Removes all transitions — good for slower devices

---

## 📦 Past Orders

1. Tap your **profile pill** → **"📦 Past Orders"**
2. See all previous orders with restaurant, people, total, date

---

## ❓ Customer App FAQ

**Q: Can I order from more than one restaurant?**
Yes! Go back to Home, pick another restaurant, add items — they merge into one cart.

**Q: What if someone doesn't want to order?**
They pay ₹0 — only people who ordered share the fees.

**Q: Is my password safe?**
Yes — SHA-256 encrypted before storing. Nobody can see your real password.

**Q: Why do I need internet?**
SplitPlate uses Firebase (Google's cloud database) for login and saving orders.

**Q: Can I remove an item after paying?**
Yes — a refund is calculated automatically and a receipt is generated.

---

---

# 📊 APP 2 — Restaurant Dashboard (`restaurant-dashboard.html`)

---

## 🔐 Login

1. Open `restaurant-dashboard.html` in Chrome
2. Select your **restaurant from the dropdown** (all 15 restaurants listed)
3. Click **"View Analytics →"**

No password needed — access is scoped to whichever restaurant you select.

---

## 📊 Dashboard Overview

Once logged in you'll see your restaurant's full analytics:

### 6 Stat Cards
| Card | What it shows |
|---|---|
| 📦 Total Orders | How many orders included your restaurant |
| 💰 Total Revenue | Sum of all your items sold |
| 🍽️ Items Sold | Total item count |
| 👥 Avg Group Size | Average number of people per order |
| 🧾 Avg Order Value | Average revenue per order at your restaurant |
| 🔥 Most Ordered Item | Your top-selling item |

### 4 Charts
| Chart | What it shows |
|---|---|
| 📊 Top Items Bar Chart | Most ordered items ranked by count |
| 💸 Revenue by Item Donut | Which items bring the most revenue |
| 📋 Order Status Donut | Confirmed / In Progress / Bill Paid breakdown |
| 📈 Orders Over Time | How many orders per day |

### Recent Orders Table
Shows last 15 orders with:
- Order ID
- Customer name
- Items from your restaurant
- Your revenue from that order
- Order status
- Time placed

---

## ❓ Restaurant Dashboard FAQ

**Q: Why does my dashboard show no data?**
No orders have been placed from your restaurant yet. Once customers order your items the data appears automatically.

**Q: Does it update in real time?**
Click **🔄 Refresh** to fetch the latest data from Firebase.

**Q: Can I see other restaurants' data?**
No — each login only shows data for the selected restaurant.

---

---

# 📈 APP 3 — Business Analytics (`splitplate-analytics.html`)

---

## 🔐 Login

1. Open `splitplate-analytics.html` in Chrome
2. Enter the admin password:

```
SplitPlate@Admin2025!
```

3. Click **"Access Dashboard →"**

> 🔒 Keep this password confidential — this dashboard shows all platform data.

---

## 📈 Dashboard Overview

### 8 KPI Cards
| Card | What it shows |
|---|---|
| 👥 Registered Users | Total accounts created on the platform |
| 📦 Total Orders | All confirmed orders across all users |
| 💰 Gross Order Value | Total money processed through the platform |
| 🏪 Active Restaurants | How many restaurants have received orders |
| 🍽️ Total Items Ordered | All items sold across all restaurants |
| 💳 Platform Fees Earned | ₹8 × number of confirmed orders |
| 🧾 Avg Order Value | Total GMV ÷ total orders |
| 🔥 Top Restaurant | Restaurant with the most orders |

> ℹ️ **Only confirmed orders** (fully placed, orderState = 'sent') count in the KPIs and charts. Orders still being built show as ₹0.

### 6 Charts
| Chart | What it shows |
|---|---|
| Revenue by Restaurant | Which restaurants earn the most |
| Orders by Restaurant | Which restaurants get the most orders |
| Orders Over Time | Daily order volume trend |
| Order Status Breakdown | Confirmed / Building / Bill Paid |
| Top Items Across Platform | Best-selling items overall |
| Group Size Distribution | How many people are in typical orders |

### 2 Tables
- **All Users** — name, email, joined date, last login, order count, total spent
- **All Orders** — every order across every user with customer, restaurants, people, total, status, time

---

## ❓ Business Analytics FAQ

**Q: Why do KPIs show 0?**
No orders have been confirmed yet. An order must be fully placed (status = 'sent') to appear in analytics.

**Q: Can I see orders that are still in progress?**
Yes — scroll to the All Orders table at the bottom. It shows all orders including ones being built.

**Q: How do I refresh the data?**
Click **🔄 Refresh** in the top right corner.

**Q: How do I lock the dashboard?**
Click **🔒 Lock** in the top right — takes you back to the password screen.

---

---

## 🛠️ General Troubleshooting (All 3 Apps)

| Problem | Solution |
|---|---|
| Firebase not loading | Check internet · Open in Chrome only |
| Can't sign in | Re-register — old plain-text accounts won't work with new hashed passwords |
| Orders not saving | Check for "🔥 Synced" toast at bottom right |
| Dashboard shows 0 | Place and fully confirm an order first |
| Text not visible | Download the latest version of the file |
| Analytics password wrong | Use `SplitPlate@Admin2025!` exactly (case sensitive) |

---

## 📁 File Structure

```
splitplate/
├── frontend/
│   ├── swiggy-split.html          ← Customer ordering app
│   ├── restaurant-dashboard.html  ← Restaurant analytics
│   └── splitplate-analytics.html ← Business BI dashboard
├── backend/
│   ├── firestore.rules
│   ├── firestore.indexes.json
│   └── README.md
└── docs/
    ├── architecture.md
    ├── project-implementation.md
    └── USER_GUIDE.md              ← This file
```

---

*SplitPlate v1.0.0 · Christ (Deemed to be University) · CIA III 2025*
*Team: Upanshu · Aanya · Vibhi · Agastya · Akankshya*
