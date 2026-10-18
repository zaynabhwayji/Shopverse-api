# Shopverse API

Shopverse is a RESTful e-commerce backend built with **Node.js, Express.js, MongoDB, and Mongoose**. It provides API endpoints for managing products, categories, tags, users, and orders, with JWT-based authentication and role-based authorization.

This project was developed for **educational purposes** to practice backend development, database relationships, REST API design, authentication, authorization, and API testing.

## 🎯 Project Goals

- Understand how a backend API works from request to response.
- Practice building RESTful APIs using Express.js.
- Work with MongoDB and Mongoose models and relationships.
- Implement CRUD operations.
- Understand authentication vs. authorization.
- Implement JWT-based authentication.
- Protect routes using middleware and user roles.
- Practice error handling and API testing with Postman.

## 🚀 Features

### 1. RESTful API

The API supports the following resources:

- **Products:** Create, read, update, and delete products.
- **Categories:** Organize products into categories.
- **Tags:** Assign tags to products.
- **Users:** Manage user information.
- **Orders:** Create and manage customer orders.

### 2. MongoDB and Mongoose

- Define schemas and models for application data.
- Create relationships between collections.
- Use references to connect related documents.
- Use `populate()` to retrieve related documents.
- Retrieve product information inside order items using nested population where configured.

### 3. CRUD Operations

The API uses standard HTTP methods:

| Method | Purpose |
|---|---|
| `GET` | Retrieve resources |
| `POST` | Create resources |
| `PATCH` / `PUT` | Update resources |
| `DELETE` | Delete resources |

The supported operations depend on the endpoint.

### 4. Filtering, Sorting, and Pagination

Product listing endpoints support query parameters for filtering, sorting, and pagination where implemented.

Examples:

```http
GET /products
GET /products?category=CATEGORY_ID
GET /products?sort=price
GET /products?page=1&limit=10
```

The exact query parameters depend on the implementation of the products controller.

### 5. JWT Authentication

Shopverse uses JSON Web Tokens (JWT) to authenticate users.

Authentication features include:

- User registration.
- User login using email and password.
- Password hashing with `bcryptjs`.
- Generating a JWT after successful login.
- Verifying tokens through authentication middleware.
- Retrieving the authenticated user's information.
- Setting an expiration time for tokens.

#### Authentication endpoints

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `POST` | `/auth/register` | Register a new user | Public |
| `POST` | `/auth/login` | Log in and receive a JWT | Public |
| `GET` | `/auth/me` | Get the authenticated user's information | Authenticated |

To access a protected endpoint, send the token in the request header:

```http
Authorization: Bearer YOUR_JWT_TOKEN
```

**Important:** A JWT is signed, not encrypted. Its payload can be read, so sensitive information such as passwords must never be stored in it.

### 6. Authorization and Roles

Authentication answers: **Who are you?**

Authorization answers: **What are you allowed to do?**

Shopverse uses role-based authorization to distinguish between users, such as:

- `customer`
- `admin`

Authentication middleware verifies the user's token. Authorization middleware checks whether the authenticated user has the required role before allowing access to a restricted route.

#### HTTP status codes

| Status | Meaning |
|---|---|
| `200 OK` | Request completed successfully |
| `201 Created` | Resource created successfully |
| `400 Bad Request` | Invalid request data |
| `401 Unauthorized` | Missing, invalid, or expired authentication |
| `403 Forbidden` | Authenticated user lacks the required permission |
| `404 Not Found` | Requested resource was not found |
| `500 Internal Server Error` | Unexpected server error |

### 7. Order Management

Orders connect users with the products they purchase.

Order-related functionality includes:

- Creating orders with user and product references.
- Storing ordered products and their quantities.
- Calculating the order total on the server.
- Associating orders with users.
- Retrieving related product and user information where implemented.
- Managing order status where supported by the controller.

The server should calculate order totals from product data rather than trusting a total supplied by the client.

### 8. Validation and Error Handling

The project practices backend error handling, including:

- Checking required request data.
- Handling invalid IDs and missing resources.
- Returning appropriate HTTP status codes.
- Handling database and server errors.
- Using centralized error-handling middleware where configured.

### 9. API Testing with Postman

Postman can be used to test the API endpoints, request bodies, responses, and authentication flow.

Testing the authentication flow:

1. Register a user using `POST /auth/register`.
2. Log in using `POST /auth/login`.
3. Copy the returned JWT.
4. Add the token to the `Authorization` header as `Bearer YOUR_JWT_TOKEN`.
5. Send a request to `GET /auth/me`.
6. Test protected routes with different user roles.
7. Check how the API responds to missing, invalid, or expired tokens and insufficient permissions.

## 🛠️ Technologies Used

- **Node.js** — JavaScript runtime.
- **Express.js** — Backend framework for building REST APIs.
- **MongoDB** — NoSQL database.
- **Mongoose** — MongoDB object modeling.
- **JSON Web Token (JWT)** — Token-based authentication.
- **bcryptjs** — Password hashing.
- **dotenv** — Environment variable configuration.
- **Postman** — API testing.
- **Nodemon** — Automatic server restart during development.

## 📁 Project Structure

```text
Shopverse/
├── controllers/
│   ├── authController.js
│   ├── categoryController.js
│   ├── productController.js
│   ├── tagController.js
│   ├── userController.js
│   └── orderController.js
├── models/
│   ├── User.js
│   ├── Product.js
│   ├── Category.js
│   ├── Tag.js
│   └── Order.js
├── routes/
│   ├── authRoutes.js
│   ├── productRoutes.js
│   ├── categoryRoutes.js
│   ├── tagRoutes.js
│   ├── userRoutes.js
│   └── orderRoutes.js
├── middleware/
│   ├── protect.js
│   └── authorize.js
├── .env
├── .gitignore
├── index.js
└── package.json
```

*The structure above is a reference; adjust filenames to match the actual project.*

## ⚙️ Getting Started

### Prerequisites

Install the following before running the project:

- [Node.js](https://nodejs.org/)
- MongoDB or a MongoDB Atlas database
- [Postman](https://www.postman.com/) for API testing

### 1. Clone the repository

```bash
git clone YOUR_REPOSITORY_URL
cd Shopverse
```

Replace `YOUR_REPOSITORY_URL` with the repository's URL.

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a `.env` file in the project's root directory:

```env
PORT=3000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_long_random_secret
```

Use the variable names expected by your code. Never commit your real `.env` file or expose your JWT secret.

### 4. Run the project

For development, if the `dev` script is configured:

```bash
npm run dev
```

To start the server using the configured start script:

```bash
npm start
```

The API is expected to be available at:

```text
http://localhost:3000
```

The actual port depends on your configuration.

## 🔐 Authentication Flow

```text
Register
   ↓
Store the user in MongoDB
   ↓
Login with email and password
   ↓
Verify the password
   ↓
Generate a signed JWT
   ↓
Send the token with protected requests
   ↓
Authentication middleware verifies the token
   ↓
Authorization middleware checks the user's role
   ↓
Controller processes the request
```

## 📚 What I Learned

Through this project, I practiced:

- Structuring a Node.js and Express backend.
- Designing RESTful API endpoints.
- Creating and connecting MongoDB models.
- Implementing CRUD operations.
- Working with references and populated documents.
- Using query parameters for filtering, sorting, and pagination.
- Hashing passwords securely.
- Implementing JWT authentication.
- Protecting routes with middleware.
- Applying role-based authorization.
- Understanding the difference between `401 Unauthorized` and `403 Forbidden`.
- Handling errors and validating request data.
- Testing API endpoints and authentication flows with Postman.

## 🎓 Project Purpose

Shopverse is an **educational project** created to strengthen practical backend development skills. Its main goal is to understand how REST APIs, databases, authentication, authorization, and middleware work together in a real-world-style e-commerce application.

It is a learning project focused on applying concepts, improving problem-solving skills, and building a foundation for more advanced full-stack applications.

## 👩‍💻 Author

**Zaynab Hwayji**
