# Airbnb Clone Backend Features and Functionalities

This section contains the key features and functionalities of the Airbnb Clone backend.

## Key Features and Functionalities

### 1. User Authentication
- **User Registration**: Sign up with first name, last name, unique email, hashed password, optional phone number, and role (guest, host, admin).
- **User Login**: Authenticate using email and password, verifying against hashed password.
- **User Roles**: Supports guest (booking, reviewing), host (property management), and admin (moderation) roles.
- **User Profile Management**: Update personal information; admins manage roles and account statuses.

### 2. Property Management
- **Property Listing**: Hosts create listings with name, description, location, and price per night.
- **Property Updates**: Hosts modify property details; updates tracked via `updated_at` timestamp.
- **Property Retrieval**: Search and filter properties by location, price, etc.

### 3. Booking System
- **Booking Creation**: Guests book properties by specifying start and end dates, with status (pending, confirmed, canceled).
- **Booking Management**: Guests cancel bookings; hosts confirm/cancel; admins manage disputes.
- **Price Calculation**: Dynamically calculate total price using `(DATEDIFF(end_date, start_date) * pricepernight)`.

### 4. Payments
- **Payment Processing**: Guests pay using credit card, PayPal, or Stripe; multiple payments per booking supported.
- **Payment Tracking**: Record amount, payment date, and method, linked to bookings.
- **Payment Verification**: Validate payments before confirming bookings.

### 5. Reviews
- **Review Submission**: Guests submit ratings (1-5) and comments for booked properties.
- **Review Retrieval**: View property reviews; aggregate ratings (e.g., average rating).
- **Review Moderation**: Admins remove inappropriate reviews.

### 6. Messaging
- **User Communication**: Guests and hosts exchange messages (e.g., availability inquiries).
- **Message Tracking**: Store message body and timestamp; retrieve message threads.
- **Message Moderation**: Admins monitor messages for policy compliance.

## Diagram
The `airbnb_clone_features.png` file is a Draw.io diagram visualizing the backend features:
- Boxes represent core components (User Authentication, Property Management, etc.).
- Arrows indicate interactions (e.g., authentication required for booking).
- Sub-features are listed within each component.

## Creation Process
The diagram was created using [dbdiagram.io](https://dbdiagram.io/d/key-features-and-functionalities-68b0a36a777b52b76c0cee3a):
1. Started a blank diagram.
2. Added boxes for each feature category with sub-features listed.
3. Connected components with arrows to show relationships.
4. Exported as `features.png` and saved in this directory.
