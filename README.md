# MedCart - Full Stack Medicine E-Commerce Application
MedCart is a robust MERN Stack (MongoDB, Express, React, Node.js) web application designed for purchasing medicines and healthcare products online. It features secure authentication, real-time cart management, Stripe payment integration, and a persistent order history system.

#  Project Architecture
The application follows a Client-Server Architecture separating the frontend user interface from the backend logic.

# High-Level Overview 
* Client Frontend: Built with React (Vite) and Bootstrap. It handles user interactions, routing, and state management via Context API.
* Server Backend: Built with Node.js and Express. It serves REST API endpoints, handles business logic, and communicates with the database.
* Database: MongoDB Atlas (Cloud) stores user data, product catalogs, and order history.
* External Services:
  * Stripe Handles secure credit card payments.
  * Google reCAPTCHA: Verifies human users during registration to prevent bots.

Directory Structure
```Plaintext
MEDCART-MAIN/
├── backend/                # Server-side logic
│   ├── config/             # Environment variables (config.env)
│   ├── database/           # DB connection logic (dbConnection.js)
│   ├── models/             # Mongoose Schemas (User, Orders, etc.)
│   ├── Routes/             # API Endpoints (Auth, Payment, Data)
│   ├── index.js            # Entry point
│   └── vercel.json         # Deployment config
└── frontend/               # Client-side UI
    ├── src/
    │   ├── components/     # Reusable UI (Navbar, Card, etc.)
    │   ├── screens/        # Pages (Home, Cart, Login, etc.)
    │   └── context/        # State Management
    ├── .env                # Frontend keys
    └── vercel.json         # Deployment config
```

# Database Schema
The application uses MongoDB with Mongoose. Below are the key collections and their structures.
## 1. `users` Collection
Stores registered user credentials.
```JavaScript
{
  "_id": ObjectId("..."),
  "name": "John Doe",
  "email": "john@example.com",
  "password": "hashed_password_string", // Bcrypt hash
  "location": "New Delhi, India",
  "date": ISODate("2023-10-25T10:00:00Z")
}
```

## 2. `orders` Collection
Stores the entire order history for a specific user.
```JavaScript
{
  "_id": ObjectId("..."),
  "email": "john@example.com", // Unique identifier
  "order_data": [
    [ // Order 1
      { "Order_date": "Mon Jan 26 2026" },
      { "id": "123", "name": "Paracetamol", "qty": 2, "price": 100 }
    ],
    [ // Order 2
      { "Order_date": "Tue Jan 27 2026" },
      { "id": "456", "name": "Vitamin C", "qty": 1, "price": 500 }
    ]
  ]
}
```

## 3. `med_items` Collection
Stores the product catalog (must be seeded manually in Atlas).
```JavaScript
{
  "_id": ObjectId("..."),
  "CategoryName": "Surgical",
  "name": "Surgical Mask",
  "img": "https://link-to-image.com/mask.jpg",
  "options": [ { "regular": "50", "large": "80" } ],
  "description": "3-ply protective mask"
}
```

## 4. `medCategory` Collection
Stores categories for filtering.
```JavaScript
{
  "_id": ObjectId("..."),
  "CategoryName": "Surgical"
}
```

# Installation & Setup
## Prerequisites
* Node.js (v14+)
* MongoDB Atlas Account
* Stripe Account (Test Mode)
* Google reCAPTCHA Admin Account

## 1. Backend Setup
Navigate to the backend folder:
```Bash
cd backend
npm install
```
### 2.Create `backend/config/config.env`:
```Code snippet
MONGO_URI=mongodb+srv://<user>:<pass>@cluster0.mongodb.net/medcart?retryWrites=true&w=majority
STRIPE_SECRET_KEY=sk_test_...
RECAPTCHA_SECRET_KEY=...
CLIENT_URL=http://localhost:5173
PORT=5000
```
### 3. Start the `server`:
```Bash
node index.js
```
## 2. Frontend Setup
Navigate to the frontend `folder`:
```Bash
cd frontend
npm install
```
Create `frontend/.env`:
```Code snippet
VITE_STRIPE_PUBLIC_KEY=pk_test_...
VITE_SITE_KEY=...
VITE_BACKEND_URL=http://localhost:5000
```
Start the app:
```Bash
npm run dev
```
# Deployment Guide (Vercel)
The project is designed to be deployed as two separate Vercel projects (Frontend & Backend).

Phase 1: Deploy Backend
* Push your code to GitHub.
* In Vercel, import the repo and set the Root Directory to backend.
* Add Environment Variables (MONGO_URI, STRIPE_SECRET_KEY, etc.).
* Deploy. Copy the resulting domain (e.g., https://medcart-api.vercel.app).

Phase 2: Deploy Frontend
* In Vercel, import the same repo again.
* Set the Root Directory to frontend.
* Add Environment Variables:
```bash
VITE_BACKEND_URL: Paste the backend domain from Phase 1.
VITE_STRIPE_PUBLIC_KEY: Your Stripe Public Key.
VITE_SITE_KEY: Your Google Site Key.
```

Phase 3: Deploy Final Config
* Go back to your Backend Project in Vercel.
* Update the CLIENT_URL variable to match your new Frontend Domain.
* Redeploy the backend.
* Add your new Frontend Domain to the Google reCAPTCHA Admin Console allowed domains list.

# API Endpoints 
Method | Endpoint | Access | Description
| :--- | :--- | :--- | :--- |
POST | /api/createUser | Public | Register user + Verify Captcha
POST | /api/loginUser | Public | Login & return Auth Token
POST | /api/foodData | Public | Get all products & categories
POST | /api/orderData | Private | Save order to history
POST | /api/myorderData | Private | Fetch user's order history
POST | /api/payment | Private | Create Stripe Checkout Session

# Author

Saurav Singh
