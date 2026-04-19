# 🪔 LivyLamp — Full-Stack E-Commerce App

A full-stack e-commerce web application for selling premium lamps, built with vanilla JavaScript, Node.js, Express, and PostgreSQL via Supabase. This project simulates a real-world online store with product browsing, user authentication, cart functionality, order management, reviews, and an admin dashboard.

---

## 🚀 Live Demo

_(Add after deployment)_

- Frontend: `https://your-frontend-url.vercel.app`
- Backend: `https://your-backend-url.onrender.com`

---

## ✨ Features

### 🛍️ E-commerce Core

- Product listing with search and filter by type
- Add to cart, update quantities, remove items
- Checkout with automatic stock decrement on order

### 🔐 Authentication

- User registration and login
- Password hashing with bcrypt
- JWT-based authentication with 7-day expiry
- Auto logout on expired or invalid token

### ⭐ Reviews

- Submit product reviews with a 1–5 star rating
- One review per user per product
- Average rating displayed per product

### 🛠️ Admin Dashboard

- View all orders and update order status
- Add, edit, and delete products
- Revenue and order statistics

---

## 🛠️ Tech Stack

| Layer       | Technology                          |
| ----------- | ----------------------------------- |
| Frontend    | HTML, CSS, Vanilla JavaScript       |
| Backend     | Node.js, Express.js                 |
| Database    | PostgreSQL (via Supabase)           |
| Auth        | JWT + bcrypt                        |
| Deployment  | Render (Backend), Vercel (Frontend) |
| API Testing | Thunder Client                      |

---

## 📂 Project Structure

```
LivyLamp/
├── frontend/
│   ├── index.html
│   ├── css/
│   │   ├── main.css          # Global styles
│   │   └── admin.css         # Admin dashboard styles
│   └── js/
│       ├── api.js            # All fetch/API calls
│       ├── ui.js             # Toast, navigation, modals
│       ├── auth.js           # Login, signup, token management
│       ├── products.js       # Product fetching, rendering, search
│       ├── cart.js           # Cart state and checkout
│       └── admin.js          # Admin dashboard logic
│
└── backend/
    ├── server.js             # Entry point, middleware, route mounting
    ├── db.js                 # PostgreSQL pool connection
    ├── middleware/
    │   └── auth.js           # authenticateToken + requireAdmin
    └── routes/
        ├── auth.js           # /register, /login
        ├── products.js       # /products, /products/:id
        ├── orders.js         # /orders
        ├── reviews.js        # /products/:id/reviews
        └── admin.js          # /admin/orders, /admin/products
```

---

## 🗄️ Database Schema

Run the following SQL in your Supabase SQL editor:

```sql
CREATE TABLE users (
  id BIGSERIAL PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  is_admin BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE products (
  id BIGSERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  description TEXT,
  price DECIMAL(10, 2) NOT NULL,
  image_url TEXT,
  stock_quantity INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE orders (
  id BIGSERIAL PRIMARY KEY,
  user_id BIGINT REFERENCES users(id) ON DELETE CASCADE,
  status TEXT DEFAULT 'pending',
  total DECIMAL(10, 2) NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE order_items (
  id BIGSERIAL PRIMARY KEY,
  order_id BIGINT REFERENCES orders(id) ON DELETE CASCADE,
  product_id BIGINT REFERENCES products(id) ON DELETE CASCADE,
  quantity INT NOT NULL,
  price DECIMAL(10, 2) NOT NULL
);

CREATE TABLE reviews (
  id BIGSERIAL PRIMARY KEY,
  product_id BIGINT REFERENCES products(id) ON DELETE CASCADE,
  user_id BIGINT REFERENCES users(id) ON DELETE CASCADE,
  rating INT CHECK (rating BETWEEN 1 AND 5),
  comment TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(product_id, user_id)
);
```

---

## ⚙️ Installation & Setup

### Prerequisites

- Node.js v18+
- A [Supabase](https://supabase.com) project with PostgreSQL

### 1. Clone the repository

```bash
git clone https://github.com/your-username/livylamp.git
cd livylamp
```

### 2. Install backend dependencies

```bash
cd backend
npm install
```

### 3. Configure environment variables

Create a `.env` file inside the `backend/` directory:

```env
PORT=3000
DATABASE_URL=your_supabase_session_pooler_connection_string
JWT_SECRET=your_secret_key_here
```

> **Important:** Use the **Session Pooler** connection string from Supabase (Settings → Database → Connection string). The Transaction Pooler will cause authentication errors.

### 4. Start the backend server

```bash
node server.js
```

The API will be running at `http://localhost:3000`.

### 5. Open the frontend

Open `frontend/index.html` directly in your browser, or serve it with a static server:

```bash
npx serve frontend
```

---

## 🔐 Admin Access

To grant admin privileges to a user, run this in your Supabase SQL editor:

```sql
UPDATE users SET is_admin = TRUE WHERE email = 'your@email.com';
```

The Admin link will appear in the navigation bar after that user logs in.

---

## 🌐 API Reference

### Auth

| Method | Route       | Auth   | Description        |
| ------ | ----------- | ------ | ------------------ |
| POST   | `/register` | Public | Create a new user  |
| POST   | `/login`    | Public | Login, returns JWT |

### Products

| Method | Route           | Auth   | Description          |
| ------ | --------------- | ------ | -------------------- |
| GET    | `/products`     | Public | Get all products     |
| GET    | `/products/:id` | Public | Get a single product |

### Reviews

| Method | Route                   | Auth     | Description              |
| ------ | ----------------------- | -------- | ------------------------ |
| GET    | `/products/:id/reviews` | Public   | Get reviews + avg rating |
| POST   | `/products/:id/reviews` | 🔒 Login | Submit a review          |

### Orders

| Method | Route     | Auth     | Description           |
| ------ | --------- | -------- | --------------------- |
| POST   | `/orders` | 🔒 Login | Place an order        |
| GET    | `/orders` | 🔒 Login | Get user's own orders |

### Admin

| Method | Route                 | Auth     | Description         |
| ------ | --------------------- | -------- | ------------------- |
| GET    | `/admin/orders`       | 🔒 Admin | View all orders     |
| PATCH  | `/admin/orders/:id`   | 🔒 Admin | Update order status |
| POST   | `/admin/products`     | 🔒 Admin | Add a product       |
| PATCH  | `/admin/products/:id` | 🔒 Admin | Edit a product      |
| DELETE | `/admin/products/:id` | 🔒 Admin | Delete a product    |
| DELETE | `/admin/reviews/:id`  | 🔒 Admin | Remove a review     |

---

## 🌍 Deployment

### Backend — Render

1. Push your repo to GitHub
2. Create a new **Web Service** on [Render](https://render.com)
3. Connect your GitHub repository
4. Set the root directory to `backend`
5. Set the start command: `node server.js`
6. Add environment variables:
   - `DATABASE_URL`
   - `JWT_SECRET`
   - `PORT`

### Frontend — Vercel

1. Create a new project on [Vercel](https://vercel.com)
2. Import your GitHub repository
3. Set the root directory to `frontend`
4. Deploy
5. Update `API_URL` in `frontend/js/api.js` to your live Render backend URL

---

## 🧪 Testing Checklist

- [ ] Backend starts without errors
- [ ] Users can register and login
- [ ] Products load from the database
- [ ] Cart add, update, and remove work correctly
- [ ] Orders are placed and saved to the database
- [ ] Stock decrements after an order
- [ ] Reviews can be submitted and viewed
- [ ] Admin can add, edit, and delete products
- [ ] Admin can view and update order status
- [ ] Expired tokens log the user out automatically

---

## 🚧 Possible Next Steps

- Input validation with `express-validator`
- Rate limiting on `/login` and `/register` with `express-rate-limit`
- HTTP security headers with `helmet.js`
- Pagination on products and orders
- User profile page with order history
- Password reset flow
- Coupon and discount code system
- Stripe payment integration
- Wishlist feature
- Email notifications

---

## 👨‍💻 Author

Ashaolu Oluwatobiloba
Adebola Omofolarin
GitHub: https://github.com/T-Tobi/LivyLamp-Ecommerce

---

## 📄 License

This project is open-source and available under the MIT License.
