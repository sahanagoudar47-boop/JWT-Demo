# JWT Authentication Demo – React + Express

A beginner-friendly **full-stack authentication project** built with **React** on the frontend and **Express** on the backend.
This project demonstrates how **JWT (JSON Web Token) authentication** works using **register**, **login**, and **protected routes**.

The backend stores users in memory for simplicity, hashes passwords with **bcryptjs**, and issues JWT tokens using **jsonwebtoken**.

---

# Features

* User registration
* User login
* Password hashing with **bcryptjs**
* JWT token generation
* Protected API route using middleware
* React frontend for login and registration
* Logout functionality
* Beginner-friendly authentication flow

---

# Tech Stack

## Frontend

* React
* CSS
* Fetch API

## Backend

* Node.js
* Express
* CORS
* bcryptjs
* jsonwebtoken

---

# Project Structure

```bash id="yq3twu"
project-folder/
│
├── backend/
│   └── index.js          # Express server with JWT authentication
│
├── frontend/
│   ├── src/
│   │   ├── App.jsx       # React frontend for login/register/profile
│   │   └── App.css       # Styling
│   └── package.json
│
└── README.md
```

---

# How the Project Works

This project has two parts:

## 1. Backend (Express API)

The backend provides routes for:

* **Registering a user**
* **Logging in**
* **Generating a JWT token**
* **Protecting a route using middleware**

## 2. Frontend (React App)

The frontend allows users to:

* Create an account
* Log in with username and password
* Store the returned JWT token in React state
* Use that token to access a protected route
* Log out

---

# Authentication Flow

## Step 1: Register

A new user enters a username and password.
The password is hashed using `bcrypt.hash()` before storing it in memory.

## Step 2: Login

The user logs in using the same credentials.
If the credentials are correct, the backend generates a **JWT token**.

## Step 3: Access Protected Route

The frontend sends the JWT token in the request header:

```http id="2lfb2d"
Authorization: Bearer <token>
```

The backend checks the token using middleware.
If valid, the user can access the protected route.

---

# Backend API Endpoints

## 1) Register a User

**Endpoint**

```http id="a0n3rm"
POST /register
```

**Request Body**

```json id="jlwm4m"
{
  "username": "sanjana",
  "password": "123456"
}
```

**Success Response**

```json id="29eqgx"
{
  "message": "Account created! You can now log in."
}
```

**Error Cases**

* Missing username/password → `400 Bad Request`
* Username already exists → `409 Conflict`

---

## 2) Login

**Endpoint**

```http id="ad4b0d"
POST /login
```

**Request Body**

```json id="0r0nha"
{
  "username": "sanjana",
  "password": "123456"
}
```

**Success Response**

```json id="u6vzqo"
{
  "token": "your_jwt_token_here"
}
```

**Error Cases**

* Invalid username → `401 Unauthorized`
* Wrong password → `401 Unauthorized`

---

## 3) Protected Profile Route

**Endpoint**

```http id="z3g3ng"
GET /profile
```

**Headers**

```http id="c4kgyn"
Authorization: Bearer your_jwt_token_here
```

**Success Response**

```json id="y0v4d1"
{
  "message": "Welcome back, sanjana!"
}
```

**Error Cases**

* No token provided → `401 Unauthorized`
* Invalid or expired token → `403 Forbidden`

---

# Backend Code Explanation

## In-Memory User Storage

The backend stores users in a simple object:

```js id="jlwm0j"
const users = {};
```

Example after registering:

```js id="0ejg11"
{
  "sanjana": {
    password: "hashed_password_here"
  }
}
```

Since this is **in-memory storage**, restarting the backend clears all registered users.

---

## Password Hashing

Passwords are hashed before storing:

```js id="4fywba"
const hashedPassword = await bcrypt.hash(password, 10);
```

This is important because storing plain-text passwords is unsafe.

---

## JWT Token Creation

When a user logs in successfully, the server creates a JWT token:

```js id="1l5ymu"
const token = jwt.sign({ username }, SECRET, { expiresIn: '1h' });
```

This token includes the username and expires in **1 hour**.

---

## Protected Route Middleware

The middleware checks the token before allowing access:

```js id="3omx3x"
function requireAuth(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ message: 'No token provided' });
  }

  jwt.verify(token, SECRET, (err, decoded) => {
    if (err) {
      return res.status(403).json({ message: 'Invalid or expired token' });
    }
    req.user = decoded;
    next();
  });
}
```

---

# Frontend Features

The React frontend includes:

* **Login/Register mode switching**
* Controlled form inputs for username and password
* API calls using `fetch()`
* Storing the JWT token in React state
* Calling a protected route with `Authorization` header
* Logout button
* Success and error messages

---

# Installation and Setup

# 1) Backend Setup

Open terminal in the backend folder and run:

```bash id="bax13m"
npm init -y
npm install express cors jsonwebtoken bcryptjs
```

Start the backend server:

```bash id="qg8x6h"
node index.js
```

If successful, you will see:

```bash id="q4usft"
Server running on http://localhost:5000
```

---

# 2) Frontend Setup

Go to the frontend React project folder and run:

```bash id="ezrd75"
npm install
npm run dev
```

The React app will usually run on:

```bash id="8szixq"
http://localhost:5173
```

---

# API Base URL Used in Frontend

The React app is configured to connect to:

```js id="njg1xk"
const API_URL = 'http://localhost:5000';
```

Make sure the backend server is running on **port 5000** before using the frontend.

---

# Example User Flow

## Register

1. Open the React app
2. Switch to **Register**
3. Enter username and password
4. Click **Create Account**
5. You will see a success message

## Login

1. Switch to **Log In**
2. Enter the same username and password
3. Click **Log In**
4. A JWT token will be returned and stored in the app

## Access Protected Route

1. Click **Get Protected Profile**
2. The frontend sends the token in the Authorization header
3. The backend verifies the token
4. If valid, it returns the welcome message

## Logout

* Click **Log Out** to clear the token from the frontend state

---

# Important Notes

* This project is for **learning/demo purposes**
* Users are stored only in memory
* Restarting the backend removes all registered users
* The JWT secret is hardcoded for simplicity
* In real applications:

  * Use a **database**
  * Store secrets in **environment variables**
  * Use **refresh tokens** if needed
  * Add validation and stronger security practices

---

# Learning Concepts Covered

This project helps you understand:

* Express route handling
* JSON request parsing with `express.json()`
* CORS setup
* Password hashing with `bcryptjs`
* JWT token creation with `jsonwebtoken`
* Authentication middleware in Express
* Protected routes
* React forms with `useState`
* Fetch API for backend communication
* Sending Authorization headers
* Basic login/register UI flow

---

# Future Improvements

You can improve this project by adding:

* MongoDB / SQLite / MySQL database
* Persistent user storage
* Form validation for password strength
* Confirm password field
* Show/hide password button
* Store JWT in localStorage or cookies
* Token expiration handling in frontend
* User dashboard page
* Profile details like email, avatar, etc.
* Better UI styling and responsiveness

---

# Security Note

This project is intentionally simple for beginners.
For production applications, do **not** store users in memory and do **not** hardcode secrets in source code.

Instead:

* Use environment variables for secrets
* Use a real database
* Add input validation
* Use HTTPS
* Consider secure cookie-based authentication when appropriate

---

# Author

Created as a beginner-friendly **JWT Authentication Demo** using **React + Express** to understand authentication flow, token handling, and protected routes.

---
