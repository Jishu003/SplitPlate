# SplitPlate — Architecture Document
**CIA III | Digital Business Systems | ECD223-3**
Christ (Deemed to be University) | B.Sc. Economics & Data Science 2025

---

## 1. Business Problem & Target Users

### Problem
When a group of friends orders food together, someone pays the full bill and then has to chase everyone else for their share. Existing platforms like Swiggy have no mechanism for group ordering with automatic bill splitting — one person must pay everything, create separate orders, or do mental arithmetic.

### Solution
SplitPlate is a group food ordering platform where each person tags their own items, the system automatically calculates each person's exact share (items + proportional fees + tax), and payment requests are sent individually via QR code.

### Target Users

| User Role | Interface | Access |
|---|---|---|
| **Customer** | `swiggy-split.html` | Sign up / Sign in |
| **Restaurant Manager** | `restaurant-dashboard.html` | Restaurant dropdown |
| **Platform Admin** | `splitplate-analytics.html` | Admin password |

---

## 2. Technology Stack

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Frontend | HTML5 + CSS3 + Vanilla JavaScript | ES2020 | All UI — no framework |
| Database | Firebase Firestore | v10.12.0 | NoSQL cloud database |
| Authentication | Custom Firestore-based | — | Email + password verification |
| Real-time sync | Firebase Firestore SDK | v10.12.0 | Live data sync |
| Fonts | Google Fonts (Inter, Playfair Display) | — | Typography |
| Charts | Pure CSS + SVG | — | Analytics visuals |
| AI Chatbot | Anthropic Claude API | claude-sonnet-4-6 | In-app support |
| Hosting | Static file (Chrome file://) | — | Current |
| Version Control | GitHub | — | Repository |

---

## 3. Current System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     SPLITPLATE SYSTEM                           │
│                                                                 │
│  ┌─────────────────┐  ┌──────────────────┐  ┌───────────────┐  │
│  │   CUSTOMER APP  │  │  RESTAURANT DASH │  │   ADMIN DASH  │  │
│  │ swiggy-split    │  │ restaurant-      │  │ splitplate-   │  │
│  │ .html           │  │ dashboard.html   │  │ analytics.html│  │
│  │                 │  │                  │  │               │  │
│  │ • Login/Signup  │  │ • Restaurant     │  │ • Platform    │  │
│  │ • 15 Restaurants│  │   selector login │  │   KPIs        │  │
│  │ • Menu + Filter │  │ • Revenue charts │  │ • GMV / fees  │  │
│  │ • Cart + Split  │  │ • Top items      │  │ • All orders  │  │
│  │ • Bill + Pay    │  │ • Order status   │  │ • User table  │  │
│  │ • QR + Refunds  │  │ • Timeline       │  │ • Analytics   │  │
│  │ • AI Chatbot    │  │                  │  │               │  │
│  └────────┬────────┘  └────────┬─────────┘  └───────┬───────┘  │
│           │                   │                     │           │
│           └───────────────────┼─────────────────────┘           │
│                               │                                 │
│              ┌────────────────▼─────────────────┐               │
│              │         FIREBASE FIRESTORE        │               │
│              │         (Cloud NoSQL DB)          │               │
│              │                                  │               │
│              │  users/{emailKey}/               │               │
│              │    orders/{orderId}/             │               │
│              │      cart, people,               │               │
│              │      perPersonDetails,           │               │
│              │      payState, grandTotal        │               │
│              └──────────────────────────────────┘               │
│                               │                                 │
│              ┌────────────────▼─────────────────┐               │
│              │      FIREBASE SDK (CDN)           │               │
│              │  firebase-app-compat.js           │               │
│              │  firebase-firestore-compat.js     │               │
│              └──────────────────────────────────┘               │
└─────────────────────────────────────────────────────────────────┘
```

---

## 4. Frontend

### 4.1 Customer App (`swiggy-split.html`)
Single-page application with 4 screens navigated via JavaScript (no page reload):

```
Screen 0 → Login / Sign Up
Screen 1 → Home (Restaurant list + Offers banner)
Screen 2 → Menu (Items, mood filter, person tagging, live calories)
Screen 3 → Cart / Bill / Pay (Split, QR, delivery tracking)
```

**Key frontend components:**
- Offers carousel with 5 animated slides
- Mood filter (All / Hungry / Lazy / Healthy / Sweet)
- Live calorie bar per person
- `[ − | qty | + ]` pill for item quantity
- Per-person bill cards with exact split
- Multi-delivery partner cards on order confirmation
- AI support chatbot (Claude API)
- Settings modal (Dark / Light / Colour Blind themes, font size, reduce motion)

### 4.2 Restaurant Dashboard (`restaurant-dashboard.html`)
Analytics portal for any of the 15 restaurants. No password — role is scoped by restaurant selection.

### 4.3 Business Analytics (`splitplate-analytics.html`)
Platform-wide BI dashboard. Password-protected (`splitplate2025`).

---

## 5. Backend / Application Logic

SplitPlate uses a **serverless backend** — Firebase Firestore replaces a traditional API server. All business logic runs client-side in JavaScript.

### Firebase SDK Integration
```javascript
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// Read
const snap = await db.collection('users').doc(emailKey).get();

// Write
await db.collection('users').doc(uid).collection('orders').doc(orderId).set(data);

// List
const snap = await db.collection('users').doc(uid).collection('orders').get();
```

---

## 6. Authentication

Custom Firestore-based authentication (no Firebase Auth SDK):

```
Sign Up:
  1. Validate email format + password length (≥6 chars)
  2. Check users/{emailKey} — reject if exists
  3. Create document with {name, email, password, createdAt, lastLogin}
  4. Set currentUser in memory → navigate to Home

Sign In:
  1. Fetch users/{emailKey}
  2. Compare password field
  3. Update lastLogin timestamp
  4. Set currentUser → navigate to Home

Email Key Formula:
  email.toLowerCase().replace(/\./g,'_').replace(/@/g,'_at_')
  e.g. jishu@gmail.com → jishu_at_gmail_com
```

> **Note:** Passwords are stored as plain text (demo scope). Production implementation should use Firebase Authentication SDK with bcrypt hashing.

---

## 7. Database

### 7.1 Firestore Collections & Entities

**Entity 1: User**
```
users/{emailKey}
  uid:        string   (email key)
  name:       string
  email:      string
  password:   string   (plain text — demo)
  createdAt:  timestamp
  lastLogin:  timestamp
```

**Entity 2: Order**
```
users/{emailKey}/orders/{orderId}
  orderId:             string
  userEmail:           string
  userName:            string
  orderState:          string  ("building"|"buffer"|"sent")
  grandTotal:          number
  subtotal:            number
  deliveryFee:         number  (40)
  platformFee:         number  (8)
  tax:                 number
  itemCount:           number
  billPaidByOrganizer: boolean
  billPaidTime:        string
  updatedAt:           timestamp
  confirmedAt:         timestamp
```

**Entity 3: Cart Item** (array within Order)
```
cart[]:
  item:      string
  price:     number
  cal:       number
  basePrice: number
  baseCal:   number
  qty:       number
  person:    string
  resto:     string
  restoId:   string
```

**Entity 4: Restaurant Group** (map within Order)
```
cartByRestaurant:
  "Meghna Foods":
    subtotal: number
    items: [{item, qty, price, person}]
```

**Entity 5: Per-Person Details** (map within Order)
```
perPersonDetails:
  "Riya":
    items:          array
    itemTotal:      number
    sharedFees:     number
    tax:            number
    totalOwed:      number
    netOwed:        number
    totalCalories:  number
    paymentStatus:  string  ("unpaid"|"qr_sent"|"paid")
    paidAt:         string
```

**Entity 6: Payment State** (map within Order)
```
payState:
  "Riya": { status: "qr_sent", time: "11:22 PM" }
  "Karan": { status: "paid", time: "11:35 PM" }
```

**Entity 7: Refund State** (map within Order)
```
refundState:
  "Riya": { refundAmt: 50, refundAt: "11:40 PM" }
```

**Entity 8: Restaurant** (static in JS)
```javascript
{
  id: string, name: string, emoji: string,
  cuisine: string, rating: number, time: string,
  badge: string, desc: string,
  menu: [{name, price, cal, mood[]}]
}
```

### 7.2 Entity Relationships

```
User ──< Order          (1 user has many orders)
Order ──< CartItem      (1 order has many cart items)
Order ──< RestaurantGroup (1 order spans many restaurants)
Order ──< PerPersonDetail (1 order has per-person breakdown)
Order ──< PaymentState  (1 order tracks each person's payment)
Order ──< RefundState   (1 order tracks refunds)
Restaurant ──< MenuItem (1 restaurant has many menu items)
Person ──< CartItem     (1 person has many tagged items)
```

### 7.3 ER Diagram

```
┌──────────┐       ┌──────────┐       ┌──────────────┐
│  User    │───<───│  Order   │───<───│  CartItem    │
│  uid PK  │       │  orderId │       │  item        │
│  name    │       │  grandTotal│     │  price       │
│  email   │       │  people[]│       │  qty         │
│  password│       │  restaurants│    │  person FK   │
│  createdAt│      │  orderState│     │  resto FK    │
└──────────┘       └────┬─────┘       └──────────────┘
                        │
          ┌─────────────┼─────────────┐
          │             │             │
    ┌─────▼──────┐ ┌────▼────────┐ ┌──▼──────────┐
    │PerPerson   │ │PayState     │ │RestaurantGrp│
    │Details     │ │status       │ │subtotal     │
    │totalOwed   │ │time         │ │items[]      │
    │tax         │ └─────────────┘ └─────────────┘
    │calories    │
    └────────────┘
```

---

## 8. Data Flow Between Components

```
1. CUSTOMER ADDS ITEM
   User clicks "Add" on menu item
   → addMenuItem() validates person selection
   → Checks cart for duplicate (same item + person) → increments qty
   → Pushes to cart[] array
   → recalcTotals() recalculates all splits
   → saveToFirebase() debounced 1000ms
   → Firestore: users/{uid}/orders/{orderId}.set(fullOrderData)

2. BILL SPLIT CALCULATION
   User clicks "Pay Full Bill"
   → payFullBillNow() shows confirmation modal
   → confirmFullPayment() sets billPaidByOrganizer = true
   → personAmount(p) = itemTotal(p) + sharedFees + tax(p)
   → QR payment cards rendered per person
   → saveToFirebase() syncs payment state

3. ORDER CONFIRMED
   User clicks "Place Order"
   → confirmOrder() groups cart by restaurant
   → Assigns random delivery partner per restaurant
   → saveFinalOrder() updates orderState: 'sent'
   → Restaurant Dashboard reads this data in real time

4. DASHBOARD READ
   Restaurant logs in → fetchOrders()
   → fsList('users') → loop all users
   → fsList('users/{uid}/orders') → filter by restaurant name
   → Aggregate: revenue, item counts, status
   → Render bar charts, donut charts, timeline, table
```

---

## 9. Current Hosting / Deployment

The application currently runs as a **static local file**:

```
User opens Chrome → Ctrl+O → selects swiggy-split.html
URL: file:///C:/Users/Upanshu/Downloads/swiggy-split.html
```

Firebase Firestore is hosted on Google Cloud (us-east1 region) and accessed via the Firebase SDK CDN loaded from `gstatic.com`.

**No web server is required** for the current demo. Firebase handles all data persistence.

---

## 10. Proposed Cloud Deployment Architecture

### AWS Deployment (Recommended for Production)

```
                        ┌─────────────────────────────┐
                        │        USERS (Global)        │
                        └──────────────┬──────────────┘
                                       │ HTTPS
                        ┌──────────────▼──────────────┐
                        │   Amazon CloudFront (CDN)   │
                        │   - Edge caching            │
                        │   - Global distribution     │
                        │   - DDoS protection         │
                        └──────────────┬──────────────┘
                                       │
                        ┌──────────────▼──────────────┐
                        │    Application Load Balancer │
                        │    (ALB)                    │
                        └──────┬───────────────┬──────┘
                               │               │
                ┌──────────────▼───┐   ┌───────▼──────────────┐
                │   AWS ECS/       │   │   AWS ECS/           │
                │   EC2 (AZ-1a)    │   │   EC2 (AZ-1b)       │
                │   Frontend       │   │   Frontend            │
                │   + App Logic    │   │   + App Logic         │
                └──────────────────┘   └──────────────────────┘
                                       │
                        ┌──────────────▼──────────────┐
                        │   Amazon DynamoDB /         │
                        │   Firebase Firestore        │
                        │   (Multi-region replication)│
                        └──────────────┬──────────────┘
                                       │
                        ┌──────────────▼──────────────┐
                        │   Amazon ElastiCache        │
                        │   (Redis — session cache)   │
                        └─────────────────────────────┘
```

### Key AWS Services

| Service | Purpose |
|---|---|
| Amazon CloudFront | CDN — serve static files globally with low latency |
| Amazon S3 | Store HTML/CSS/JS files |
| Application Load Balancer | Distribute traffic across EC2/ECS instances |
| Amazon ECS (Fargate) | Container orchestration — auto-scaling |
| Amazon DynamoDB | Managed NoSQL database (replaces Firestore at scale) |
| Amazon ElastiCache (Redis) | Session caching, rate limiting |
| Amazon Cognito | Production authentication with JWT tokens |
| Amazon WAF | Web application firewall |
| Amazon CloudWatch | Monitoring and alerting |
| Amazon Route 53 | DNS management |
| AWS Certificate Manager | SSL/TLS certificates |

---

## 11. Scalability to 1 Million Users

### Architecture Changes at 1M Users

**Application Layer:**
- Deploy minimum 10 ECS Fargate containers behind ALB
- Auto-scaling group: scale out when CPU > 60%
- Read replicas for database

**Database Layer:**
- Firebase Firestore handles 1M users natively (tested at scale by Google)
- Add composite indexes for frequent queries (updatedAt + restaurantName)
- Enable Firestore caching for repeated reads

**CDN Layer:**
- CloudFront caches all static assets at 200+ edge locations
- Cache-Control headers: 1 year for HTML/CSS/JS

**Estimated Infrastructure:**
```
CloudFront:        ~$0.085/GB transfer
ECS Fargate:       10 tasks × t3.medium = ~$150/month
Firestore:         ~$0.06/100K reads = ~$60/month at 100M reads
ElastiCache:       cache.t3.micro = ~$15/month
Total estimate:    ~$500-800/month at 1M users
```

---

## 12. Scalability to 5 Million Users

### Architecture Changes at 5M Users

**Application Layer:**
- Move to Kubernetes (Amazon EKS) for pod-level auto-scaling
- Separate microservices: Auth Service, Order Service, Analytics Service
- Message queue (Amazon SQS) for async Firebase writes
- WebSocket server for real-time order status updates

**Database Layer:**
- Shard Firestore by geography (India, SEA, Global)
- Add DynamoDB as secondary store for analytics aggregations
- Redis cluster for hot data (active orders, session tokens)
- Read-through cache for restaurant menus (rarely change)

**Network Layer:**
- Multiple AWS regions: Mumbai (ap-south-1), Singapore (ap-southeast-1)
- Global Accelerator for routing to nearest region
- Rate limiting: 1000 req/min per user

**Estimated Infrastructure:**
```
EKS cluster:       50+ pods = ~$2,000/month
Firestore:         ~$300/month at 500M reads
DynamoDB:          ~$500/month for analytics
ElastiCache:       Redis cluster = ~$200/month
CloudFront:        ~$400/month
Total estimate:    ~$5,000-8,000/month at 5M users
```

---

## 13. Quantitative Scalability Analysis

### 13.1 User Growth Projection (25% annual growth)

**Formula:** Uₙ = U₀ × (1 + r)ⁿ where U₀ = 10,000, r = 0.25

| Year | Formula | Users |
|---|---|---|
| Year 0 | 10,000 × (1.25)⁰ | **10,000** |
| Year 1 | 10,000 × (1.25)¹ | **12,500** |
| Year 2 | 10,000 × (1.25)² | **15,625** |
| Year 3 | 10,000 × (1.25)³ | **19,531** |
| Year 4 | 10,000 × (1.25)⁴ | **24,414** |
| Year 5 | 10,000 × (1.25)⁵ | **30,518** |

**Interpretation:** At 25% annual growth starting from 10,000 users, SplitPlate reaches ~30,500 users in 5 years. To reach 1M users would require either faster growth (40%+ annually) or a marketing push.

### 13.2 Peak Concurrent Users (10% of registered)

**Formula:** Peak Concurrent = Registered Users × 0.10

| Registered Users | Formula | Peak Concurrent |
|---|---|---|
| 100,000 | 100,000 × 0.10 | **10,000** |
| 500,000 | 500,000 × 0.10 | **50,000** |
| 1,000,000 | 1,000,000 × 0.10 | **100,000** |
| 5,000,000 | 5,000,000 × 0.10 | **500,000** |

**Interpretation:** At 1M registered users, 100,000 users are active simultaneously during peak. This requires significant horizontal scaling — at least 20–50 application servers.

### 13.3 Requests Per Minute and Per Second (5 req/min per active user)

**Formula:** RPM = Active Users × 5 | RPS = RPM ÷ 60

| Active Users | Formula (RPM) | RPM | Formula (RPS) | RPS |
|---|---|---|---|---|
| 10,000 | 10,000 × 5 | **50,000** | 50,000 ÷ 60 | **~833** |
| 50,000 | 50,000 × 5 | **250,000** | 250,000 ÷ 60 | **~4,167** |
| 100,000 | 100,000 × 5 | **500,000** | 500,000 ÷ 60 | **~8,333** |
| 500,000 | 500,000 × 5 | **2,500,000** | 2,500,000 ÷ 60 | **~41,667** |

**Interpretation:**
- At 10,000 active users: 833 RPS — a single well-optimised server handles this (typical limit ~1,000–5,000 RPS)
- At 50,000 active users: 4,167 RPS — requires load balancer + 3–5 servers
- At 100,000 active users: 8,333 RPS — requires 10+ servers or auto-scaling containers
- At 500,000 active users: 41,667 RPS — requires distributed architecture with caching, CDN and 50+ containers

---

## 14. Security Mechanisms

| # | Security Mechanism | Component | Purpose | Threat Addressed |
|---|---|---|---|---|
| 1 | **Email + Password Authentication** | Login screen | Verifies user identity before granting access | Unauthorised access |
| 2 | **Email Key Sanitisation** | Firebase path | Removes `.` and `@` from document IDs to prevent path injection | Firestore path injection |
| 3 | **Input Validation** | Sign Up form | Validates email format (regex), password length ≥6 chars, non-empty name | SQL/NoSQL injection, weak passwords |
| 4 | **Debounced Firebase Writes** | saveToFirebase() | 1000ms debounce prevents write flooding | Firebase quota exhaustion / DoS |
| 5 | **Role-Based Access** | 3 dashboards | Customer, Restaurant, Admin have separate entry points and scoped data | Privilege escalation |
| 6 | **Try/Catch Error Handling** | All Firebase calls | Prevents crashes from exposing stack traces to users | Information disclosure |
| 7 | **Firestore Security Rules** | Firebase backend | Restricts read/write access to authenticated paths | Unauthorised data access |
| 8 | **HTTPS (Firebase CDN)** | SDK loading | All Firebase SDK and API calls made over HTTPS | Man-in-the-middle attacks |
| 9 | **Admin Password Gate** | Analytics dashboard | Separates business analytics from public-facing interfaces | Unauthorised data exposure |
| 10 | **Colour Blind Mode** | UI | Ensures status is not communicated by colour alone | Accessibility / misread status |

---

## 15. Failure and Recovery Analysis

| Failure | Impact | Detection | Recovery |
|---|---|---|---|
| **Application/Server** — HTML file corrupted or fails to load | All 3 UIs inaccessible | User sees blank page or parse error | Restore from GitHub repository; CDN re-serve |
| **Database** — Firestore outage or quota exceeded | Orders not saved; login fails | `saveToFirebase()` catch block fires; toast shows "Sync failed" | Firebase has 99.999% SLA; auto-recovers; implement local storage fallback |
| **Network** — User loses internet mid-order | Firebase writes fail silently | Toast notification "Sync failed"; console error | Debounced retry on next user action; order state preserved in JS memory until page reload |
| **Storage** — Firebase storage limits reached | New writes rejected with quota error | Console logs Firebase quota error | Upgrade Firebase plan; implement cleanup of orders older than 90 days |
| **Security** — Admin password compromised | Analytics dashboard accessible to unauthorised party | Unusual access pattern in Firebase logs | Change admin password in source; rotate Firebase API key in GCP console; enable Firebase App Check |

---

*SplitPlate · CIA III · Digital Business Systems ECD223-3 · Christ (Deemed to be University) · 2025*
