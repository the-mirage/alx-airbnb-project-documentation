# Airbnb Clone Backend Requirements

This document outlines the technical and functional requirements for three key backend features of the Airbnb Clone: User Authentication, Property Management, and Booking System. Each feature includes API endpoints, input/output specifications, validation rules, and performance criteria, aligning with the schema in `database-script-0x02/schema.sql` and features in `features-and-functionalities/`.

## 1. User Authentication

### Functional Requirements
- **Purpose**: Allow users to register, log in, update profiles, and enable admins to manage user accounts.
- **User Stories**:
  - As a user, I want to register an account with my email, password, and personal details so that I can access the platform as a guest, host, or admin.
  - As a user, I want to log in with my email and password so that I can access my account.
  - As a user, I want to update my profile so that my personal information is current.
  - As an admin, I want to manage users so that I can moderate accounts and roles.

### Technical Requirements
- **API Endpoints**:
  - **POST /api/users/register**
    - **Description**: Register a new user.
    - **Input** (JSON):
      ```json
      {
        "first_name": "string (max 50 chars)",
        "last_name": "string (max 50 chars)",
        "email": "string (valid email, max 100 chars)",
        "password": "string (min 8 chars, max 255 chars)",
        "phone_number": "string (optional, max 20 chars)",
        "role": "string (enum: guest, host, admin)"
      }
      ```
      - Example:
        ```json
        {
          "first_name": "John",
          "last_name": "Doe",
          "email": "john.doe@example.com",
          "password": "securePass123",
          "phone_number": "555-0101",
          "role": "guest"
        }
        ```
    - **Output**:
      - Success (201 Created):
        ```json
        {
          "user_id": "UUID",
          "email": "string",
          "role": "string",
          "message": "User registered successfully"
        }
        ```
      - Error (400 Bad Request, 409 Conflict):
        ```json
        {
          "error": "Invalid input data" | "Email already exists"
        }
        ```
    - **Validation Rules**:
      - `first_name`, `last_name`: Required, non-empty, max 50 chars.
      - `email`: Required, valid email, unique in Users table.
      - `password`: Required, min 8 chars, hashed (e.g., bcrypt).
      - `phone_number`: Optional, max 20 chars, valid phone format.
      - `role`: Required, must be `guest`, `host`, or `admin`.
  - **POST /api/users/login**
    - **Description**: Authenticate user and issue JWT.
    - **Input** (JSON):
      ```json
      {
        "email": "string (valid email)",
        "password": "string"
      }
      ```
      - Example:
        ```json
        {
          "email": "john.doe@example.com",
          "password": "securePass123"
        }
        ```
    - **Output**:
      - Success (200 OK):
        ```json
        {
          "user_id": "UUID",
          "email": "string",
          "role": "string",
          "token": "JWT string"
        }
        ```
      - Error (401 Unauthorized):
        ```json
        {
          "error": "Invalid email or password"
        }
        ```
    - **Validation Rules**:
      - `email`: Required, valid email.
      - `password`: Required, must match hashed password.
  - **PUT /api/users/{user_id}**
    - **Description**: Update user profile (authenticated user).
    - **Input**:
      - Headers: Authorization: Bearer `<JWT>`
      - Body (JSON):
        ```json
        {
          "first_name": "string (optional, max 50 chars)",
          "last_name": "string (optional, max 50 chars)",
          "phone_number": "string (optional, max 20 chars)"
        }
        ```
    - **Output**:
      - Success (200 OK):
        ```json
        {
          "user_id": "UUID",
          "message": "Profile updated successfully"
        }
        ```
      - Error (400 Bad Request, 401 Unauthorized):
        ```json
        {
          "error": "Invalid input data" | "Unauthorized"
        }
        ```
    - **Validation Rules**:
      - User must be authenticated (valid JWT).
      - Only the user or admin can update the profile.
      - Fields are optional but must meet requirements if provided.
  - **PUT /api/users/{user_id}/role** (Admin only)
    - **Description**: Update user role.
    - **Input**:
      - Headers: Authorization: Bearer `<JWT>` (admin required)
      - Body (JSON):
        ```json
        {
          "role": "string (enum: guest, host, admin)"
        }
        ```
    - **Output**:
      - Success (200 OK):
        ```json
        {
          "user_id": "UUID",
          "role": "string",
          "message": "Role updated successfully"
        }
        ```
      - Error (400 Bad Request, 401 Unauthorized, 403 Forbidden):
        ```json
        {
          "error": "Invalid role" | "Unauthorized" | "Admin access required"
        }
        ```
    - **Validation Rules**:
      - Caller must have `admin` role.
      - `role`: Must be `guest`, `host`, or `admin`.

- **Performance Criteria**:
  - Response Time: < 200ms for login/register (1000 concurrent users).
  - Scalability: Handle 10,000 registrations/logins per hour.
  - Security: Passwords hashed with bcrypt; JWTs with 1-hour expiration.
  - Error Rate: < 1% failure rate for valid requests.
  - Data Integrity: Unique email constraint; atomic transactions.

## 2. Property Management

### Functional Requirements
- **Purpose**: Enable hosts to create/update property listings; allow guests to search properties.
- **User Stories**:
  - As a host, I want to create a property listing so that guests can book it.
  - As a host, I want to update my property listings to keep information current.
  - As a guest, I want to search properties by location or price to find a place to stay.

### Technical Requirements
- **API Endpoints**:
  - **POST /api/properties**
    - **Description**: Create a property listing (host only).
    - **Input**:
      - Headers: Authorization: Bearer `<JWT>` (host required)
      - Body (JSON):
        ```json
        {
          "name": "string (max 100 chars)",
          "description": "string",
          "location": "string (max 255 chars)",
          "pricepernight": "number (decimal, min 0.01)"
        }
        ```
    - **Output**:
      - Success (201 Created):
        ```json
        {
          "property_id": "UUID",
          "name": "string",
          "message": "Property created successfully"
        }
        ```
      - Error (400 Bad Request, 401 Unauthorized, 403 Forbidden):
        ```json
        {
          "error": "Invalid input data" | "Unauthorized" | "Host access required"
        }
        ```
    - **Validation Rules**:
      - Caller must have `host` role.
      - `name`, `description`, `location`, `pricepernight`: Required.
      - `name`: Max 100 chars.
      - `location`: Max 255 chars.
      - `pricepernight`: Positive decimal (2 decimal places).
  - **PUT /api/properties/{property_id}**
    - **Description**: Update a property listing (host only).
    - **Input**:
      - Headers: Authorization: Bearer `<JWT>` (host required)
      - Body (JSON):
        ```json
        {
          "name": "string (optional, max 100 chars)",
          "description": "string (optional)",
          "location": "string (optional, max 255 chars)",
          "pricepernight": "number (optional, decimal, min 0.01)"
        }
        ```
    - **Output**:
      - Success (200 OK):
        ```json
        {
          "property_id": "UUID",
          "message": "Property updated successfully"
        }
        ```
      - Error (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found):
        ```json
        {
          "error": "Invalid input data" | "Unauthorized" | "Host access required" | "Property not found"
        }
        ```
    - **Validation Rules**:
      - Caller must be the property’s host.
      - Fields are optional but must meet requirements if provided.
  - **GET /api/properties**
    - **Description**: Search properties with filters.
    - **Input**:
      - Query Parameters: `location` (string), `min_price` (number), `max_price` (number), `page` (integer, default 1), `limit` (integer, default 10)
    - **Output**:
      - Success (200 OK):
        ```json
        {
          "properties": [
            {
              "property_id": "UUID",
              "host_id": "UUID",
              "name": "string",
              "description": "string",
              "location": "string",
              "pricepernight": "number",
              "created_at": "timestamp",
              "updated_at": "timestamp"
            }
          ],
          "total": "number",
          "page": "number",
          "limit": "number"
        }
        ```
      - Error (400 Bad Request):
        ```json
        {
          "error": "Invalid query parameters"
        }
        ```
    - **Validation Rules**:
      - `location`: Optional, case-insensitive.
      - `min_price`, `max_price`: Optional, positive decimals, `min_price` <= `max_price`.
      - `page`, `limit`: Positive integers.

- **Performance Criteria**:
  - Response Time: < 300ms for create/update; < 500ms for search (1000 properties).
  - Scalability: Handle 1000 create/updates, 10,000 searches per hour.
  - Data Integrity: Validate `host_id`; atomic transactions.
  - Error Rate: < 1% failure rate.
  - Indexing: Indexes on `property_id`, `host_id`, `location`.

## 3. Booking System

### Functional Requirements
- **Purpose**: Allow guests to create/manage bookings, hosts to confirm/cancel bookings, and calculate total price dynamically.
- **User Stories**:
  - As a guest, I want to book a property by specifying start and end dates to secure a place to stay.
  - As a host, I want to manage bookings to control my property’s availability.

### Technical Requirements
- **API Endpoints**:
  - **POST /api/bookings**
    - **Description**: Create a booking (guest only).
    - **Input**:
      - Headers: Authorization: Bearer `<JWT>` (guest required)
      - Body (JSON):
        ```json
        {
          "property_id": "UUID",
          "start_date": "YYYY-MM-DD",
          "end_date": "YYYY-MM-DD"
        }
        ```
    - **Output**:
      - Success (201 Created):
        ```json
        {
          "booking_id": "UUID",
          "property_id": "UUID",
          "user_id": "UUID",
          "start_date": "YYYY-MM-DD",
          "end_date": "YYYY-MM-DD",
          "status": "pending",
          "total_price": "number",
          "message": "Booking created, awaiting host confirmation"
        }
        ```
      - Error (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict):
        ```json
        {
          "error": "Invalid dates" | "Unauthorized" | "Guest access required" | "Property not found" | "Property unavailable"
        }
        ```
    - **Validation Rules**:
      - Caller must have `guest` role.
      - `property_id`: Must exist in Properties.
      - `start_date`, `end_date`: Valid dates, `end_date` > `start_date`, future dates.
      - Availability: No overlapping `confirmed` bookings.
      - Total price: `(DATEDIFF(end_date, start_date) * pricepernight)`.
  - **PUT /api/bookings/{booking_id}/status**
    - **Description**: Update booking status (host or admin).
    - **Input**:
      - Headers: Authorization: Bearer `<JWT>` (host/admin required)
      - Body (JSON):
        ```json
        {
          "status": "string (enum: confirmed, canceled)"
        }
        ```
    - **Output**:
      - Success (200 OK):
        ```json
        {
          "booking_id": "UUID",
          "status": "string",
          "message": "Booking status updated"
        }
        ```
      - Error (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found):
        ```json
        {
          "error": "Invalid status" | "Unauthorized" | "Host/Admin access required" | "Booking not found"
        }
        ```
    - **Validation Rules**:
      - Caller must be the property’s host or admin.
      - `status`: Must be `confirmed` or `canceled`.
  - **GET /api/bookings**
    - **Description**: Retrieve bookings (guest, host, admin).
    - **Input**:
      - Headers: Authorization: Bearer `<JWT>`
      - Query Parameters: `user_id` (UUID), `property_id` (UUID), `status` (enum), `page` (integer, default 1), `limit` (integer, default 10)
    - **Output**:
      - Success (200 OK):
        ```json
        {
          "bookings": [
            {
              "booking_id": "UUID",
              "property_id": "UUID",
              "user_id": "UUID",
              "start_date": "YYYY-MM-DD",
              "end_date": "YYYY-MM-DD",
              "status": "string",
              "total_price": "number",
              "created_at": "timestamp"
            }
          ],
          "total": "number",
          "page": "number",
          "limit": "number"
        }
        ```
      - Error (400 Bad Request, 401 Unauthorized):
        ```json
        {
          "error": "Invalid query parameters" | "Unauthorized"
        }
        ```
    - **Validation Rules**:
      - Guest: View own bookings.
      - Host: View bookings for their properties.
      - Admin: View all bookings.
      - `status`: Optional, must be `pending`, `confirmed`, or `canceled`.

- **Performance Criteria**:
  - Response Time: < 300ms for creation; < 500ms for retrieval (1000 records).
  - Scalability: Handle 5000 bookings, 10,000 queries per hour.
  - Data Integrity: Foreign key constraints; atomic transactions for availability.
  - Error Rate: < 1% failure rate.
  - Indexing: Indexes on `booking_id`, `property_id`, `user_id`, `status`.

