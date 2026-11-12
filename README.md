# Vendora — Full-Stack E-Commerce Platform (`e-com_`)

A complete, modern, full-stack TypeScript e-commerce platform featuring a customer-facing storefront and a comprehensive administrative dashboard with real-time analytics, inventory management, order fulfilment, and review moderation.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
  - [Customer Storefront](#customer-storefront)
  - [Admin Management Dashboard](#admin-management-dashboard)
  - [Security & Architecture](#security--architecture)
- [Demo Credentials](#-demo-credentials)
- [Tech Stack](#-tech-stack)
- [Project Layout](#-project-layout)
- [Database Schema](#-database-schema)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [1. Database Setup](#1-database-setup)
  - [2. Backend Setup](#2-backend-setup)
  - [3. Frontend Setup](#3-frontend-setup)
- [Environment Configuration](#-environment-configuration)
- [Available Scripts](#-available-scripts)
- [REST API Reference](#-rest-api-reference)
  - [Authentication (`/api/auth`)](#authentication-apiauth)
  - [Storefront (`/api`)](#storefront-api)
  - [Admin (`/api/admin`)](#admin-apiadmin)

---

## 🌟 Overview

**Vendora** (`e-com_`) is built from the ground up with TypeScript across the entire stack. It provides a seamless shopping experience for customers and a powerful command center for store administrators.

```
e-com_/
├── client/          # React 18 + Vite + TypeScript (Storefront & Admin UI)
├── server/          # Node.js + Express + TypeScript REST API (Port 5000)
└── database/        # MySQL Schema & Initial Setup (ecom_db)
```

---

## ✨ Key Features

### Customer Storefront
- **Catalog Navigation**: Browse products with category and nested subcategory filtering, keyword search, price display, and real-time average star ratings.
- **Product Details**: High-resolution imagery, rich descriptions, real-time inventory stock indicators, customer reviews, and official admin responses.
- **Interactive Reviews**: Authenticated customers can submit star ratings (1–5) and written feedback for purchased products.
- **Cart & Wishlist**: Persistent cart state and synchronized user wishlists with one-click toggling.
- **Transactional Checkout**: Atomic checkout process ensuring database transaction integrity and real-time stock deduction.
- **Order Tracking**: Order history dashboard showing item breakdowns, order status, shipping addresses, and exact timestamps.

### Admin Management Dashboard
- **KPI Metrics & Analytics**: Real-time summary cards for total revenue, orders, registered users, and active products.
- **Recharts Visualizations**: Interactive charts tracking monthly revenue trends, 14-day daily sales volume, and order distributions by status.
- **Product Management**: Full CRUD operations with modal interfaces for creating, updating, pricing, categorizing, and managing stock levels.
- **Category & Subcategory Hierarchy**: Manage product taxonomies with automated parent-child category links.
- **Order Fulfilment Workflow**: Full lifecycle order status management (`Pending` → `Processing` → `Shipped` → `Delivered` / `Cancelled`).
- **User & Role Administration**: Manage registered customer accounts and administrator privileges, with built-in safeguards against self-deletion.
- **Review Moderation**: Moderate customer reviews and post official administrative replies.

### Security & Architecture
- **JWT Authentication**: Secure JSON Web Tokens with Axios request/response interceptors and automated token expiration handling.
- **Password Hashing**: Strong bcryptjs password hashing with salt rounds.
- **Role-Based Access Control (RBAC)**: Route guards (`ProtectedRoute`, `AdminRoute`) on the client and Express middleware (`verifyToken`, `isAdmin`) on the API.
- **Parameterized SQL Queries**: All database queries use parameterized placeholders (`?`) to prevent SQL injection vulnerabilities.
- **Foreign Key Cascade Deletion**: Relational integrity enforced at the database level with `ON DELETE CASCADE`.

---

## 🔑 Demo Credentials

The database seeder automatically initializes demo accounts with pre-populated orders, products, and reviews:

| Role | Phone Number | Password | Landing Page | Access Permissions |
| :--- | :--- | :--- | :--- | :--- |
| **Admin** | `9999999999` | `admin123` | `/admin` | Full Storefront + Admin Dashboard |
| **Customer** | `8888888888` | `user123` | `/` | Customer Storefront, Cart, Orders & Wishlist |

> **Tip**: The Login page features a **Quick Demo Login** panel with one-click **"Fill Dummy Admin Login"** and **"Fill Dummy User Login"** buttons for immediate testing.

---

## 🛠 Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Frontend** | React 18, Vite 5, TypeScript (`.tsx`), Tailwind CSS, React Router DOM 6, Axios, Lucide React, Recharts |
| **Backend** | Node.js, Express 4, TypeScript, `tsx` runtime, `mysql2/promise` connection pool, bcryptjs, jsonwebtoken, CORS, dotenv |
| **Database** | MySQL 8.x / MariaDB (8 normalized relational tables with foreign keys and cascade rules) |

---

## 📂 Project Layout

```
e-com_/
├── database/
│   └── schema.sql                  # MySQL database initialization script
│
├── server/                         # Express REST API
│   ├── .env.example                # Sample environment variables
│   ├── package.json
│   ├── tsconfig.json
│   └── src/
│       ├── index.ts                # Express server entry point (:5000)
│       ├── seed.ts                 # Idempotent database seeder script
│       ├── config/
│       │   └── db.ts               # mysql2/promise connection pool
│       ├── middleware/
│       │   └── authMiddleware.ts   # verifyToken and isAdmin guards
│       ├── routes/
│       │   ├── auth.ts             # Signup, login, profile (/api/auth)
│       │   ├── categories.ts       # Categories & subcategories (/api/categories)
│       │   ├── products.ts         # Products catalog & reviews (/api/products)
│       │   ├── wishlist.ts         # User wishlist (/api/wishlist)
│       │   ├── orders.ts           # Order placement & history (/api/orders)
│       │   └── admin.ts            # Admin dashboard & management (/api/admin)
│       └── types/
│           └── index.ts            # TypeScript interfaces & database row types
│
└── client/                         # React + Vite Client
    ├── index.html
    ├── package.json
    ├── tailwind.config.js
    ├── tsconfig.json
    ├── vite.config.ts              # Vite config with API proxy to :5000
    └── src/
        ├── App.tsx                 # Route configuration & app shell
        ├── main.tsx                # React DOM root mounting
        ├── api/
        │   └── client.ts           # Axios instance with auth interceptors
        ├── context/
        │   ├── AuthContext.tsx     # Authentication state & persistent tokens
        │   ├── CartContext.tsx     # Shopping cart state & local persistence
        │   └── WishlistContext.tsx # Wishlist state & backend sync
        ├── components/
        │   ├── Navbar.tsx          # Storefront navigation bar
        │   ├── ProductCard.tsx     # Product grid card component
        │   ├── Modal.tsx           # Reusable modal for admin CRUD forms
        │   ├── StatusBadge.tsx     # Color-coded badge for order statuses
        │   ├── StarRating.tsx      # Interactive/display star ratings
        │   ├── ProtectedRoute.tsx  # Customer authentication route wrapper
        │   └── AdminRoute.tsx      # Admin role-verification route wrapper
        ├── layouts/
        │   ├── StoreLayout.tsx     # Storefront layout (navbar + content container)
        │   └── AdminLayout.tsx     # Admin sidebar navigation & header layout
        └── pages/
            ├── HomePage.tsx           # Product catalog, filters, and search
            ├── ProductDetailsPage.tsx # Detailed view, reviews, and add to cart
            ├── CartPage.tsx           # Cart management and checkout flow
            ├── WishlistPage.tsx       # Saved items view
            ├── OrdersPage.tsx         # Customer order history
            ├── LoginPage.tsx          # Login form with quick demo autofill
            ├── SignupPage.tsx         # New customer registration
            └── admin/
                ├── AdminDashboard.tsx  # KPIs, Recharts sales analytics
                ├── AdminProducts.tsx   # Product management (CRUD)
                ├── AdminCategories.tsx # Categories and subcategories management
                ├── AdminOrders.tsx     # Order status updates and fulfillment
                ├── AdminUsers.tsx      # User accounts and role moderation
                └── AdminReviews.tsx    # Customer review moderation and replies
```

---

## 🗄 Database Schema

The database `ecom_db` is defined in `database/schema.sql` and includes 8 interconnected tables:

```
 users (id, full_name, phone_no, address, state, pin_code, password_hash, role)
   │
   ├──< orders (id, user_id, total_amount, shipping_address, status)
   │      └──< order_items (id, order_id, product_id, quantity, price)
   │
   ├──< wishlist (id, user_id, product_id)
   │
   └──< reviews (id, product_id, user_id, rating, comment, admin_reply)

 categories (id, name, description, image_url)
   │
   └──< subcategories (id, category_id, name)
          │
          └──< products (id, category_id, subcategory_id, name, description, price, stock, image_url)
```

- **`users`**: Unique phone numbers, bcrypt password hashes, and `role` ENUM (`customer`, `admin`).
- **`categories` & `subcategories`**: Hierarchical classification with `ON DELETE CASCADE`.
- **`products`**: Linked to categories and subcategories; tracks price and inventory stock.
- **`wishlist`**: Unique constraint on `(user_id, product_id)` to prevent duplicate entries.
- **`orders`**: Order tracking with status ENUM (`Pending`, `Processing`, `Shipped`, `Delivered`, `Cancelled`).
- **`order_items`**: Line items linked to orders, capturing price at the time of purchase.
- **`reviews`**: Enforces `CHECK (rating BETWEEN 1 AND 5)`, with optional `admin_reply`.

---

## 🚀 Getting Started

### Prerequisites

Ensure you have the following installed on your local environment:
- **Node.js** (v18.0.0 or higher)
- **npm** (v9.0.0 or higher)
- **MySQL** (v8.0 or higher) or **MariaDB** (v10.5 or higher)

---

### 1. Database Setup

1. Start your local MySQL server.
2. Run the SQL schema to create the `ecom_db` database and all 8 tables:

```bash
mysql -u root -p < database/schema.sql
```

---

### 2. Backend Setup

1. Navigate to the server directory:

```bash
cd server
```

2. Install dependencies:

```bash
npm install
```

3. Create and configure your environment file:

```bash
cp .env.example .env
```

Open `.env` and verify your MySQL connection settings:

```env
PORT=5000
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_mysql_password
DB_NAME=ecom_db
JWT_SECRET=your_super_secret_jwt_key
JWT_EXPIRES_IN=7d
```

4. Seed the database with demo users, sample catalog, orders, and reviews:

```bash
npm run seed
```

5. Start the backend development server:

```bash
npm run dev
```

The Express API will be running at **http://localhost:5000**.

---

### 3. Frontend Setup

1. Open a new terminal window and navigate to the client directory:

```bash
cd client
```

2. Install dependencies:

```bash
npm install
```

3. Start the Vite development server:

```bash
npm run dev
```

4. Open **http://localhost:5173** in your browser.
   - Use the **Quick Demo Login** button to sign in as **Admin** or **Customer**.
   - Browse products, add items to cart, test checkout, or visit `/admin` to explore analytics and management features.

---

## ⚙️ Environment Configuration

### Server Environment Variables (`server/.env`)

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `PORT` | `5000` | HTTP port the Express server listens on |
| `DB_HOST` | `localhost` | MySQL host address |
| `DB_PORT` | `3306` | MySQL port number |
| `DB_USER` | `root` | MySQL database username |
| `DB_PASSWORD` | *(empty)* | MySQL database password |
| `DB_NAME` | `ecom_db` | MySQL database name |
| `JWT_SECRET` | `change_this_to_a_long_random_secret` | Secret key used to sign and verify JSON Web Tokens |
| `JWT_EXPIRES_IN` | `7d` | Token expiration duration (e.g. `7d`, `24h`) |

---

## 📜 Available Scripts

### Backend (`server/`)

| Command | Description |
| :--- | :--- |
| `npm run dev` | Runs the server in development mode with hot-reloading via `tsx watch` |
| `npm run seed` | Runs `src/seed.ts` to populate sample categories, products, orders, and demo users |
| `npm run build` | Compiles TypeScript code to plain JavaScript in `dist/` |
| `npm start` | Executes the compiled production bundle (`dist/index.js`) |
| `npm run typecheck` | Checks TypeScript types without emitting files (`tsc --noEmit`) |

### Frontend (`client/`)

| Command | Description |
| :--- | :--- |
| `npm run dev` | Starts the Vite development server at `http://localhost:5173` |
| `npm run build` | Runs type checking and builds the production bundle in `dist/` |
| `npm run preview` | Locally previews the production build |
| `npm run typecheck` | Checks TypeScript types without emitting files (`tsc --noEmit`) |

---

## 📡 REST API Reference

All endpoints accept and return `application/json`. Authenticated routes require the header:
```
Authorization: Bearer <your_jwt_token>
```

### Authentication (`/api/auth`)

| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/signup` | Public | Register a new customer account (`full_name`, `phone_no`, `address`, `state`, `pin_code`, `password`, `confirm_password`) |
| `POST` | `/api/auth/login` | Public | Authenticate with `phone_no` and `password`; returns JWT token and user profile |
| `GET` | `/api/auth/me` | 🔒 Authenticated | Retrieve profile information for the authenticated user |

### Storefront (`/api`)

| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/categories` | Public | Retrieve all categories with nested subcategories |
| `GET` | `/api/products` | Public | List products with optional query filters: `?category=`, `?subcategory=`, `?search=` |
| `GET` | `/api/products/:id` | Public | Get product details, average rating, and customer reviews |
| `POST` | `/api/products/:id/reviews` | 🔒 Authenticated | Submit a product review with `rating` (1–5) and `comment` |
| `GET` | `/api/wishlist` | 🔒 Authenticated | Retrieve all products in the current user's wishlist |
| `POST` | `/api/wishlist/:productId` | 🔒 Authenticated | Add a product to the user's wishlist |
| `DELETE` | `/api/wishlist/:productId` | 🔒 Authenticated | Remove a product from the user's wishlist |
| `POST` | `/api/orders` | 🔒 Authenticated | Create a new order with atomic stock deduction |
| `GET` | `/api/orders/my-orders` | 🔒 Authenticated | Retrieve order history for the current user |

### Admin (`/api/admin`)

> 🔒 Requires JWT authentication **and** `role === 'admin'`.

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/api/admin/stats` | Retrieve revenue, orders, users, products KPIs, monthly sales, daily sales, and status breakdowns |
| `GET` | `/api/admin/users` | List all registered user accounts |
| `PUT` | `/api/admin/users/:id` | Update user details or toggle `role` (`customer` / `admin`) |
| `DELETE` | `/api/admin/users/:id` | Delete user account (self-deletion blocked) |
| `POST` | `/api/admin/categories` | Create a new product category |
| `PUT` | `/api/admin/categories/:id` | Update category name, description, or image |
| `DELETE` | `/api/admin/categories/:id` | Delete a category (cascades to subcategories and products) |
| `POST` | `/api/admin/subcategories` | Create a new subcategory linked to a category |
| `PUT` | `/api/admin/subcategories/:id` | Update subcategory name or parent category |
| `DELETE` | `/api/admin/subcategories/:id` | Delete a subcategory |
| `POST` | `/api/admin/products` | Create a new product with category, price, and stock |
| `PUT` | `/api/admin/products/:id` | Update product details, price, inventory stock, or image |
| `DELETE` | `/api/admin/products/:id` | Delete a product |
| `GET` | `/api/admin/orders` | List all customer orders with customer details and line items |
| `PUT` | `/api/admin/orders/:id/status` | Update order status (`Pending`, `Processing`, `Shipped`, `Delivered`, `Cancelled`) |
| `GET` | `/api/admin/reviews` | List all customer reviews across all products |
| `PUT` | `/api/admin/reviews/:id` | Update review or submit an official `admin_reply` |
| `DELETE` | `/api/admin/reviews/:id` | Remove an inappropriate review |

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
