<div align="center">

#  Fruits-Zone
### Enterprise Agro & Organic E-Commerce Platform

[![Node.js](https://img.shields.io/badge/Node.js-v18%2B-green.svg?style=for-the-badge&logo=node.js)](https://nodejs.org)
[![React](https://img.shields.io/badge/React-19.0-61dafb.svg?style=for-the-badge&logo=react)](https://react.dev)
[![Vite](https://img.shields.io/badge/Vite-8.0-646cff.svg?style=for-the-badge&logo=vite)](https://vitejs.dev)
[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-v4.0-38bdf8.svg?style=for-the-badge&logo=tailwindcss)](https://tailwindcss.com)
[![Express](https://img.shields.io/badge/Express-4.21-black.svg?style=for-the-badge&logo=express)](https://expressjs.com)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47a248.svg?style=for-the-badge&logo=mongodb)](https://mongodb.com)

**A high-performance, full-stack seasonal agro & organic product e-commerce ecosystem designed for high-conversion pre-bookings, flexible delivery management, and real-time storefront administration.**

[Key Features](#-key-features) • [Architecture](#-architecture) • [Getting Started](#-getting-started) • [User Manual](#-user-manual) • [Admin Guide](#-admin-guide) • [API Reference](#-api-reference) • [Security](#-security--hardening) • [Deployment](#-deployment)

---

</div>

## 📖 Overview

**Fruits-Zone (Ammwala)** is an enterprise-grade web application built to solve the operational and conversion bottlenecks of seasonal organic food businesses in Bangladesh. Specially tailored for Rajshahi/Chapai mangoes, Sundarban raw honey, Dinajpur lychees, and cold-pressed mustard oils, it combines frictionless pre-ordering with powerful administrative controls.

### Product Categories Supported
- **🥭 Mangoes** — GI-certified varieties (Himsagar, Gopalbhog, Langra, Amrapali, Fazli)
- **🍯 Pure Honey** — Sundarban Kholisa, Mustard flower honey
- **🍒 Lychees** — Dinajpur Bedana & China-3 varieties
- **🫒 Oils & Ghee** — Cold-pressed mustard oil, traditional ghee

---

## ✨ Key Features

### 🛍️ High-Converting Storefront

| Feature | Description |
|---------|-------------|
| **SEO-Friendly URLs** | Dynamic slugs (e.g., `/product/khirsapati-himsagar`) instead of raw IDs |
| **Clean Navigation** | Smooth section scrolling without hash fragments in URL bar |
| **Delivery Location Selector** | Dynamic Dhaka / Outside Dhaka with auto-calculated charges |
| **Free Delivery Override** | Per-product free delivery badges with automatic ৳0 fee calculation |
| **Interactive Product Details** | Weight/package toggles, harvest schedules, storage tips |
| **Direct WhatsApp Ordering** | One-click pre-populated WhatsApp chat with order details |
| **Customer Invoice Modal** | Print-optimized single-page invoice with transaction ID |
| **Responsive Design** | Mobile-first, works on all devices |

### 📊 Real-Time Administrative Suite (`/admin`)

| Module | Capabilities |
|--------|--------------|
| **Dashboard** | Today's metrics, revenue, visitors, recent orders, product snapshot |
| **Products** | CRUD with live preview, image badges, scheduled publishing, draft auto-save |
| **Categories** | Manage product categories with icons, units, badges |
| **Orders/Bookings** | Status lifecycle (Pending→Confirmed→Shipped→Delivered), search, WhatsApp/call |
| **Reviews** | Approve/reject customer reviews |
| **Settings** | **Visual storefront builder** — Header, Hero, Product Section, About, Testimonials, Footer |

### 🎨 Visual Storefront Builder (Admin Settings)
1. **Header & Navigation** — Brand display modes, call/WhatsApp buttons, custom nav links with icons
2. **Hero Banner** — Multi-slide carousel, trust badges, hotline numbers
3. **Product Section** — Grid/Carousel/Hide, card styling presets
4. **About Section** — Founder portrait, narrative, live/custom statistics
5. **Testimonials** — Custom curated or live verified reviews, auto-play controls
6. **Footer** — 4-column layout, feature highlights, social links, return policy

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      FRONTEND (React 19 + Vite)                │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │ Storefront  │  │  Admin UI   │  │ AppContext  │            │
│  │  (Public)   │  │  (/admin)   │  │ (Global State)          │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘            │
│         │                │                │                    │
│         └────────────────┼────────────────┘                    │
│                          ▼                                    │
│              ┌───────────────────────┐                        │
│              │  API Service Layer    │                        │
│              │  (services/api.js)    │                        │
│              └───────────┬───────────┘                        │
└──────────────────────────┼────────────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                    VITE REVERSE PROXY                           │
│              /api  ──►  http://localhost:5000/api              │
└──────────────────────────┼────────────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                     BACKEND (Node.js + Express)                │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│  │ Helmet   │ │  CORS    │ │Rate Limit│ │  JWT     │          │
│  │ Security │ │  Policy  │ │  (300/15m)│ │  Auth    │          │
│  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘          │
│       │            │            │            │                 │
│       └────────────┼────────────┼────────────┘                 │
│                    ▼            ▼                               │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Controllers: Auth │ Category │ Product │ Order │ Review  │  │
│  └────────────────────────────┬──────────────────────────────┘  │
│                               ▼                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Mongoose Models: User, Category, Product, Order, Review,│  │
│  │  Setting, Visitor                                        │  │
│  └────────────────────────────┬──────────────────────────────┘  │
└───────────────────────────────┼─────────────────────────────────┘
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                    MONGODB DATABASE                             │
│  (Auto-seeds: Admin, Categories, Products, Settings, Orders)   │
│  In-memory fallback if MongoDB unavailable                     │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Getting Started

### Prerequisites
- **Node.js**: v18.0.0 or higher
- **npm**: v9.0.0 or higher
- **MongoDB**: Local instance or MongoDB Atlas URI *(Optional — in-memory fallback activates automatically if no database found)*

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/hasanuzzamanpriyam/Fruits-Zone.git
cd Fruits-Zone

# 2. Install backend dependencies
cd backend
npm install

# 3. Install frontend dependencies
cd ../frontend
npm install
```

### Environment Setup

Create `backend/.env` in the backend directory:

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# Database
MONGO_URI=mongodb://127.0.0.1:27017/ammwala_db

# Authentication
JWT_SECRET=your_super_secure_jwt_secret_key_2026
JWT_EXPIRES_IN=7d

# Admin Credentials (change in production!)
ADMIN_EMAIL=admin@test.com
ADMIN_PASSWORD=Test123@#

# Frontend URL (for CORS)
FRONTEND_URL=http://localhost:5173
```

### Running Development Servers

#### Terminal 1 — Backend
```bash
cd backend
npm run dev
# Server running at: http://localhost:5000/api
# Health check: http://localhost:5000/api/health
```

#### Terminal 2 — Frontend
```bash
cd frontend
npm run dev
# Storefront: http://localhost:5173
# Admin Panel: http://localhost:5173/admin/login
```

---

## 📋 User Manual

This section provides a complete guide for **customers** (storefront users) and **administrators** (admin panel users).

---

### 🛒 Customer Storefront Guide

#### 1. **Homepage Navigation**
The homepage (`/`) is a single-page layout with smooth scrolling sections:
- **Hero Banner** — Featured seasonal products with CTA
- **Mango Varieties** — Click any variety to pre-select in order form
- **About Us** — Company story and trust signals
- **Testimonials** — Verified customer reviews
- **Order Form** — Place pre-bookings at the bottom

#### 2. **Browsing Products**
- Navigate to **Product Details** by clicking any product card
- URL format: `/product/{slug}` (e.g., `/product/khirsapati-himsagar`)
- **Product Page Features:**
  - Image gallery with thumbnails
  - Package size selector (5kg, 10kg, 20kg crates)
  - Region-based harvest schedule with calendar popover
  - Taste specifications (sweetness, fiber, aroma)
  - Storage & ripening guide
  - **Delivery Location Selector** — Choose "Inside Dhaka" or "Outside Dhaka"

#### 3. **Placing an Order (Pre-Booking)**

**Step 1:** Select your delivery location
- **ঢাকার ভিতর (Inside Dhaka)** — ৳120 delivery charge
- **ঢাকার বাইরে (Outside Dhaka)** — ৳150 delivery charge
- *Free delivery products show ৳0 automatically*

**Step 2:** Choose package & quantity
- Select package size (e.g., "৫ কেজি মাস্টার বক্স")
- Adjust quantity (minimum order quantity enforced)

**Step 3:** Fill customer details
- Full Name (required)
- Phone Number (required, BD format)
- Delivery Address (required)
- Optional: Delivery date preference, special notes

**Step 4:** Choose payment method
- **Cash on Delivery (COD)** — Pay when delivered
- **bKash** — Mobile banking
- **Nagad** — Mobile banking

**Step 5:** Submit order
- Click "অর্ডার নিশ্চিত করুন" (Confirm Order)
- Receive Order ID (format: `BK-XXXXX`)
- Order confirmation sent via WhatsApp/phone

#### 4. **WhatsApp Quick Order**
- Click the floating WhatsApp button (bottom-right)
- Or use "WhatsApp-এ অর্ডার" button on product page
- Pre-fills: Product, package, quantity, address, calculated total
- Opens WhatsApp chat with formatted order message

#### 5. **Viewing Invoice**
- After order submission, click "ইনভয়েস দেখুন" (View Invoice)
- Print-optimized single-page invoice with:
  - Transaction ID & date
  - Customer & delivery details
  - Itemized billing with delivery charge
  - Company branding
- Use browser print (Ctrl+P) for physical copy

---

### 🔐 Admin Panel Guide

#### Access
- **URL:** `http://localhost:5173/admin/login` (or `/login`)
- **Default Credentials:**
  - Email: `admin@test.com`
  - Password: `Test123@#`
- **Role:** `superadmin` (full access)

#### Dashboard (`/admin`)
Real-time overview with 8 metric cards:
| Card | Description |
|------|-------------|
| Today's Pre-Bookings | Orders placed today + pending count |
| Today's Revenue | Paid orders revenue today |
| Today's Visitors | Unique visitors today |
| Total Visitors | All-time visitor count |
| Total Pre-Bookings | All orders + confirmed count |
| Pending Orders | Orders needing action |
| Total Revenue | All-time paid revenue |
| Active Products | Live products in catalog |

**Quick Actions:** "Add New Product", "View Orders"

**Recent Orders Table:** Last 5 orders with inline status update dropdown

**Product Snapshot:** First 4 products with quick edit links

---

#### Products Management (`/admin/products`)

**List View:**
- Search by name (Bengali/English)
- Filter by category, stock status
- Pagination (10 per page)
- Actions: Edit, Delete, Toggle Featured

**Add/Edit Product (`/admin/products/add` or `/edit/:id`):**

| Section | Fields |
|---------|--------|
| **Basic Info** | Bengali Name*, English Name, Subtitle, Category*, Unit* |
| **Pricing** | Price/Unit*, Regular Price, Discount %, Min Order (kg/qty) |
| **Stock** | Status (In Stock/Pre-Order/Out), Featured toggle |
| **Images** | Main image, Gallery (multiple), Image Badges (4 corners) |
| **Origin & Schedule** | Origin, Harvest Schedule, Region Dates |
| **Description** | Rich text (TinyMCE), Highlights (bullet points) |
| **Taste Specs** | Label/Value pairs (e.g., Sweetness: 22° Brix) |
| **Packages** | Multiple package options with kg, label, price, badge |
| **Storage Tips** | 3-step guide (unboxing, fridge, chemical-free signs) |
| **Packaging Steps** | 3-step process (grading, cushioning, delivery) |
| **SEO** | Meta title, description, keywords |
| **Delivery** | Free delivery toggle, Dhaka/Outside charges |
| **Publishing** | Draft auto-save (5s), Schedule future publish date |

**Live Preview:** Toggle Desktop/Mobile view to see storefront appearance

**Image Badge Customizer:**
- Drag badges to any corner (top-left, top-right, bottom-left, bottom-right)
- Resize with drag handles
- Font size control (10-24px)
- Color presets (green, red, yellow, blue)
- Types: "normal", "hot", "new", "sale"

---

#### Categories Management (`/admin/categories`)
- Add/Edit/Delete categories
- Fields: Bengali/English name, Slug, Icon emoji, Unit, Available units, Badge, Description, Status, Display order

---

#### Orders/Bookings Management (`/admin/bookings`)
- **Search:** Customer name, phone, Order ID
- **Filters:** Status, Date range, Delivery area
- **Actions per order:**
  - Update status: Pending → Confirmed → Shipped → Delivered / Cancelled
  - Call customer (tel: link)
  - WhatsApp customer (pre-filled message)
  - View full details modal
- **Export:** Print order list

**Order Status Lifecycle:**
```
Pending (new) → Confirmed (accepted) → Shipped (dispatched) → Delivered
                    ↓
              Cancelled (any time)
```

---

#### Reviews Management (`/admin/reviews`)
- View all customer reviews
- Approve/Reject (toggles `isApproved` field)
- Approved reviews appear on storefront testimonials
- Filter by approval status, rating

---

#### Settings / Storefront Builder (`/admin/settings`)

**Visual, real-time configuration with live preview.**

##### 1. Header & Navigation
| Setting | Options |
|---------|---------|
| Brand Display | Logo Only / Logo & Text / Text Only |
| Site Name | Text input |
| Tagline | Text input |
| Title Color | Solid color / Gradient (color picker) |
| Call Button | Show/Hide, Custom text, Phone number |
| WhatsApp Button | Show/Hide, Number |
| Nav Links | Add/Remove/Reorder with custom icons/emojis |

##### 2. Hero Banner
- Multiple slides (add/remove/reorder)
- Per slide: Image, Title, Subtitle, Button text/link
- Trust badges (array of text)
- Hotline numbers

##### 3. Product Section
| Setting | Options |
|---------|---------|
| Display Mode | Grid View / Auto Carousel / Hide Section |
| Section Title | Custom text |
| Card Style | Border radius, Shadow, Hover effect presets |
| Card Preset | Minimal / Standard / Detailed |

##### 4. About Section
- Founder portrait upload
- Narrative description (rich text)
- Statistics source: **Live Database** or **Custom Numbers**
  - Custom: Total Orders, Varieties, Satisfaction %

##### 5. Testimonials
| Setting | Options |
|---------|---------|
| Source | Custom Curated / Live Verified Reviews |
| Auto-play | Enable/Disable, Speed slider (ms) |
| Custom Reviews | Add with: Name, Location, Rating, Text, Product link |

##### 6. Footer & Features
- 4-column layout: About, Quick Links, Contact, Newsletter
- Color scheme: Background, Text, Accent (live pickers)
- Social links: Facebook, Instagram, YouTube, WhatsApp
- Feature highlights (toggle): Nationwide Delivery, Quality Assurance, 24/7 Support, Easy Returns
- Return policy text
- Copyright notice

---

## 🔌 API Reference

### Base URL
```
Development: http://localhost:5000/api
Production:  https://your-domain.com/api
```

### Authentication
- **Type:** Bearer Token (JWT)
- **Header:** `Authorization: Bearer <token>`
- **Login:** `POST /api/auth/login` → returns `{ token, user }`
- **Token Expiry:** 7 days

### Rate Limits
- **Global API:** 300 requests / 15 minutes
- **Auth endpoints:** 20 requests / 15 minutes

---

### 📦 Products

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/products` | No | List products (query: category, search, page, limit, featured) |
| GET | `/products/featured` | No | Get featured products |
| GET | `/products/:slug` | No | Get product by slug |
| GET | `/products/id/:id` | No | Get product by MongoDB ID |
| POST | `/products` | Admin | Create product |
| PUT | `/products/:id` | Admin | Update product |
| DELETE | `/products/:id` | Admin | Delete product |
| PATCH | `/products/:id/featured` | Admin | Toggle featured status |

**Query Parameters (List):**
- `category` — Category slug (mango, honey, lychee, oil-ghee)
- `search` — Text search in name/description
- `featured` — `true`/`false`
- `page` — Page number (default: 1)
- `limit` — Items per page (default: 10)

---

### 📁 Categories

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/categories` | No | List all categories |
| GET | `/categories/:slug` | No | Get category by slug |
| POST | `/categories` | Admin | Create category |
| PUT | `/categories/:id` | Admin | Update category |
| DELETE | `/categories/:id` | Admin | Delete category |

---

### 📋 Orders (Bookings)

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/orders` | Admin | List orders (query: status, search, page, limit, dateFrom, dateTo) |
| GET | `/orders/stats` | Admin | Dashboard statistics |
| GET | `/orders/:id` | Admin | Get order details |
| POST | `/orders` | No | Create order (customer) |
| PATCH | `/orders/:id/status` | Admin | Update order status |
| PATCH | `/orders/:id/payment` | Admin | Update payment status |
| DELETE | `/orders/:id` | Admin | Delete order |

**Order Status Values:** `Pending`, `Confirmed`, `Shipped`, `Delivered`, `Cancelled`

**Payment Status Values:** `Pending`, `Paid`, `Unpaid`

**Payment Methods:** `cod`, `bkash`, `nagad`

---

### ⭐ Reviews

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/reviews` | No | List approved reviews |
| GET | `/reviews/all` | Admin | List all reviews (pending + approved) |
| POST | `/reviews` | No | Submit review (customer) |
| PATCH | `/reviews/:id/approve` | Admin | Approve/reject review |
| DELETE | `/reviews/:id` | Admin | Delete review |

---

### ⚙️ Settings

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/settings` | No | Get public settings (storefront) |
| GET | `/settings/admin` | Admin | Get full settings (admin) |
| PUT | `/settings` | Admin | Update settings |

---

### 👥 Visitors (Analytics)

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/visitors/stats` | Admin | Get visitor stats (today/total) |
| POST | `/visitors/track` | No | Track page visit |

---

### 🔐 Authentication

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/auth/login` | No | Admin login |
| GET | `/auth/me` | Yes | Get current user profile |
| POST | `/auth/logout` | Yes | Logout (client-side token removal) |

---

## 🛡️ Security & Hardening

| Layer | Protection | Implementation |
|-------|------------|----------------|
| **Authentication** | JWT Bearer Tokens | Cryptographically signed, 7-day expiry, validated against DB |
| **Authorization** | Role-Based Access (RBAC) | Admin endpoints reject non-admin/unauthenticated with 401 |
| **Rate Limiting** | DoS & Brute-Force Prevention | Global: 300 req/15min; Auth: 20 req/15min |
| **Header Security** | Helmet.js | XSS, Clickjacking, MIME-sniffing, Cross-origin protection |
| **CORS** | Origin Validation | Dynamic origin verification, supports dev tunnels |
| **Password Security** | Bcrypt (12 rounds) | Hashed before persistence, excluded from queries by default |
| **Input Validation** | Mongoose Strict Schemas | NoSQL injection prevention, type casting, trimming |

### Environment Security Checklist (Production)
- [ ] Change `JWT_SECRET` to 64+ char random string
- [ ] Change `ADMIN_EMAIL` / `ADMIN_PASSWORD`
- [ ] Use MongoDB Atlas with IP whitelist
- [ ] Enable HTTPS (SSL/TLS)
- [ ] Set `NODE_ENV=production`
- [ ] Configure `FRONTEND_URL` to exact domain
- [ ] Set up MongoDB authentication
- [ ] Enable database backups

---

## 🌐 Deployment

### Production Build
```bash
cd frontend
npm run build
# Output: frontend/dist/ (static assets, Gzip/Brotli ready)
```

### Deployment Options

#### Vercel / Netlify (Frontend)
1. Connect GitHub repo
2. Build command: `cd frontend && npm run build`
3. Output directory: `frontend/dist`
4. Add environment variables

#### VPS / Cloud VM (Full Stack)
```bash
# Backend (PM2)
cd backend
npm install -g pm2
pm2 start src/server.js --name "ammwala-api"
pm2 startup
pm2 save

# Frontend (Nginx)
# Copy frontend/dist/* to /var/www/html/
# Configure Nginx reverse proxy for /api to localhost:5000
```

#### Docker (Optional)
```dockerfile
# Backend Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY backend/package*.json ./
RUN npm ci --production
COPY backend/ .
EXPOSE 5000
CMD ["node", "src/server.js"]
```

### Cloudflare Tunnel (Live Preview)
```bash
# From project root
./cloudflared.exe tunnel --url http://localhost:5173
# Outputs: https://xxxx.trycloudflare.com
```

---

## 📁 Project Structure

```
organic-product-ecommerce-/
├── backend/
│   ├── package.json
│   ├── src/
│   │   ├── server.js              # Express app entry
│   │   ├── config/
│   │   │   └── db.js              # MongoDB connection + in-memory fallback
│   │   ├── models/
│   │   │   ├── User.js            # Admin users
│   │   │   ├── Category.js        # Product categories
│   │   │   ├── Product.js         # Products (rich schema)
│   │   │   ├── Order.js           # Orders/Bookings
│   │   │   ├── Review.js          # Customer reviews
│   │   │   ├── Setting.js         # Storefront settings
│   │   │   └── Visitor.js         # Analytics
│   │   ├── routes/
│   │   │   ├── authRoutes.js
│   │   │   ├── categoryRoutes.js
│   │   │   ├── productRoutes.js
│   │   │   ├── orderRoutes.js
│   │   │   ├── reviewRoutes.js
│   │   │   ├── settingRoutes.js
│   │   │   └── visitorRoutes.js
│   │   ├── controllers/
│   │   ├── middlewares/
│   │   │   ├── authMiddleware.js
│   │   │   ├── errorMiddleware.js
│   │   │   └── rateLimiter.js
│   │   └── seeders/
│   │       └── seed.js            # Auto-seeds admin, categories, products
│   └── seed_orders.js
│
├── frontend/
│   ├── package.json
│   ├── vite.config.js
│   ├── index.html
│   ├── public/
│   │   ├── favicon.svg
│   │   ├── icons.svg
│   │   └── assets/
│   │       └── mangoes/           # Local product images
│   └── src/
│       ├── main.jsx               # React entry + providers
│       ├── App.jsx                # Routes + layout
│       ├── App.css
│       ├── index.css              # Tailwind + global styles
│       ├── context/
│       │   └── AppContext.jsx     # Global state (cart, products, settings)
│       ├── services/
│       │   └── api.js             # Axios API client
│       ├── components/
│       │   ├── Navbar.jsx
│       │   ├── Footer.jsx
│       │   ├── Hero.jsx
│       │   ├── MangoVarieties.jsx
│       │   ├── About.jsx
│       │   ├── Testimonials.jsx
│       │   ├── OrderForm.jsx
│       │   ├── OrderInvoiceModal.jsx
│       │   ├── FloatingSocial.jsx
│       │   ├── common/            # Reusable UI components
│       │   └── admin/             # Admin-specific components
│       ├── pages/
│       │   ├── HomePage.jsx
│       │   ├── SingleProductPage.jsx
│       │   ├── NotFound.jsx
│       │   └── admin/
│       │       ├── AdminLoginPage.jsx
│       │       ├── AdminLayout.jsx
│       │       ├── AdminDashboard.jsx
│       │       ├── AdminProductList.jsx
│       │       ├── AdminProductForm.jsx
│       │       ├── AdminCategories.jsx
│       │       ├── AdminBookingsList.jsx
│       │       ├── AdminReviewsList.jsx
│       │       └── AdminSettings.jsx
│       ├── utils/
│       │   └── productUtils.js
│       └── data/
│           ├── products.js        # Fallback static data
│           └── orders.js
```

---

## 🧪 Development Commands

### Backend
```bash
cd backend
npm run dev      # Development with --watch
npm run start    # Production
npm run seed     # Manual database seeding
```

### Frontend
```bash
cd frontend
npm run dev      # Vite dev server
npm run build    # Production build
npm run preview  # Preview production build
npm run lint     # Oxlint code quality
```

---

## 🤝 Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

## 📄 License & Attribution

This project is proprietary and actively maintained for **Fruits-Zone (Ammwala Agro & Organic Foods)**. All rights reserved.

---

## 📞 Support & Contact

- **Website:** https://fruits-zone.com (configure in settings)
- **Admin Email:** admin@fruits-zone.com
- **WhatsApp:** +880 17XX-XXXXXX (configure in settings)
- **GitHub Issues:** https://github.com/hasanuzzamanpriyam/Fruits-Zone/issues

---

<div align="center">

**Built with ❤️ for Bangladesh's Organic Agriculture**

*Empowering farmers, delivering freshness, building trust.*

</div>
