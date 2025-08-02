# Flask Application (app.py)

## Overview
This is a Flask-based web application that provides user authentication, product management, and order placement functionality. It uses MySQL as the database and JWT for authentication.

## Features
- User authentication (register & login) with JWT-based authentication.
- Secure password hashing using SHA-256.
- Product listing and order placement.
- Database models for users, products, and orders.
- Routes for user authentication, product retrieval, and order placement.

## Flask Application Details

### API Endpoints

#### 1️⃣ User Authentication
- **Register a user**
  ```sh
  POST /register
  ```
  **Request Body:**
  ```json
  {
    "email": "user@example.com",
    "username": "user1",
    "password": "securepassword"
  }
  ```
  **Response:**
  ```json
  { "message": "User registered successfully!" }
  ```

- **Login a user**
  ```sh
  POST /login
  ```
  **Request Body:**
  ```json
  {
    "email": "user@example.com",
    "password": "securepassword"
  }
  ```
  **Response:**
  ```json
  { "token": "jwt_token_here" }
  ```

#### 2️⃣ Products
- **Retrieve all products**
  ```sh
  GET /products
  ```
  **Response:**
  ```json
  [
    { "id": 1, "name": "Product 1", "price": 10.99, "stock": 50 },
    { "id": 2, "name": "Product 2", "price": 5.49, "stock": 100 }
  ]
  ```

#### 3️⃣ Orders
- **Add product to cart**
  ```sh
  POST /add-to-cart
  ```
  **Request Body:**
  ```json
  {
    "user_id": 1,
    "product_id": 2,
    "quantity": 3
  }
  ```
  **Response:**
  ```json
  { "message": "Item added to cart!" }
  ```

- **Checkout order**
  ```sh
  POST /checkout
  ```
  **Response:**
  ```json
  { "message": "Order placed successfully!" }
  ```

## Security Considerations
- Uses JWT for authentication.
- Passwords are hashed using SHA-256.
- `umask 077` is used in the Dockerfile to ensure strict file permissions.
- The application should use environment variables for secret keys in production.

## Running the Application
To start the application:
```sh
python app.py
```

## Logs & Debugging
- View application logs:
  ```sh
  docker compose logs app
  ```
- Access the running container:
  ```sh
  docker exec -it <container_id> sh
  ```

## Future Enhancements
- Improve authentication by using Flask-JWT-Extended.
- Implement product purchase tracking.
- Enhance error handling and validation.
- Move database credentials to a `.env` file for better security.
- Implement CI/CD pipeline for automated deployment.
