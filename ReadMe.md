# 💡 Lamp Store — Full Stack E-commerce App

A full-stack e-commerce web application for selling lamps.  
This project simulates a real-world online store with product browsing, user authentication, cart functionality, and order management.

---

## 🚀 Live Demo
*(Add after deployment)*  
Frontend: https://your-frontend-url.vercel.app  
Backend: https://your-backend-url.onrender.com  

---

## 📖 Overview

**Lamp Store** is designed to demonstrate modern full-stack development practices.  
Users can browse products, search and filter lamps, add items to a cart, and complete purchases.  

The application includes authentication, persistent data storage, and scalable backend APIs.

---

## ✨ Features

### 🛍️ E-commerce Core
- Product listing page
- Product details page
- Add to cart functionality
- Cart management (update/remove items)
- Checkout and order creation

### 🔐 Authentication
- User registration
- User login
- Password hashing (bcrypt)
- JWT-based authentication

### 🔍 Advanced Features
- Search functionality
- Category filtering
- Product reviews and ratings
- Image upload support
- Admin product management

---

## 🛠️ Tech Stack

### Frontend
- HTML
- CSS
- JavaScript (Vanilla)
- Fetch API

### Backend
- Node.js
- Express.js

### Database
- PostgreSQL (Supabase)

### Authentication
- JSON Web Tokens (JWT)
- bcrypt

### Deployment
- Render (Backend)
- Vercel (Frontend)

---

## 📂 Project Structure

```
lamp-store/
│
├── frontend/       # UI (HTML, CSS, JS)
│
├── backend/        # API (Node.js + Express)
│   ├── routes/
│   ├── middleware/
│   ├── db.js
│   └── server.js
│
└── README.md
```

---

## ⚙️ Installation & Setup

### 1. Clone the repository
```bash
git clone https://github.com/your-username/lamp-store.git
cd lamp-store
```

---

### 2. Backend Setup
```bash
cd backend
npm install
```

Create a `.env` file inside `/backend`:

```
DATABASE_URL=your_supabase_connection_string
JWT_SECRET=your_secret_key
PORT=3000
```

Run backend:
```bash
node server.js
```

---

### 3. Frontend Setup
```bash
cd ../frontend
```

Open `index.html` in your browser  
(or use Live Server)

---

## 🗄️ Database Setup (Supabase)

Run the following SQL in Supabase:

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
  name TEXT,
  description TEXT,
  price NUMERIC,
  category TEXT,
  image_url TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE cart (
  id BIGSERIAL PRIMARY KEY,
  user_id BIGINT,
  product_id BIGINT,
  quantity INT
);

CREATE TABLE orders (
  id BIGSERIAL PRIMARY KEY,
  user_id BIGINT,
  total NUMERIC,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE order_items (
  id BIGSERIAL PRIMARY KEY,
  order_id BIGINT,
  product_id BIGINT,
  quantity INT,
  price NUMERIC
);

CREATE TABLE reviews (
  id BIGSERIAL PRIMARY KEY,
  user_id BIGINT,
  product_id BIGINT,
  rating INT,
  comment TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 🔌 API Endpoints

### Auth
- `POST /register`
- `POST /login`

### Products
- `GET /products`
- `GET /products/:id`
- `GET /products/search?q=&category=`

### Cart
- `GET /cart`
- `POST /cart`
- `PUT /cart/:id`
- `DELETE /cart/:id`

### Orders
- `POST /orders`
- `GET /orders`

### Reviews
- `POST /reviews`

---

## 🌍 Deployment

### Backend (Render)
- Connect GitHub repo
- Set start command:
```bash
node server.js
```
- Add environment variables:
  - DATABASE_URL
  - JWT_SECRET

---

### Frontend (Vercel)
- Import frontend folder
- Deploy
- Update API base URL to production backend

---

## 🧪 Testing Checklist

- [ ] Backend runs without errors  
- [ ] Users can register and login  
- [ ] Products load correctly  
- [ ] Cart functionality works  
- [ ] Orders are created successfully  
- [ ] Data persists after refresh  

---

## 🔥 Future Improvements

- Stripe payment integration  
- Responsive mobile UI  
- Admin dashboard UI  
- Wishlist feature  
- Email notifications  
- Product inventory tracking  

---

## 👨‍💻 Author

Your Name  
GitHub: https://github.com/your-username  

---

## 📄 License

This project is open-source and available under the MIT License.
