# Airbnb Clone Backend Data Flow Diagram

## Data Flow Diagram Overview

The DFD is a Level-1 diagram showing interactions between external entities (Guest, Host, Admin), processes, data stores, and data flows for core backend operations.

### External Entities
- **Guest**: Initiates bookings, payments, reviews, and messages.
- **Host**: Manages properties, confirms/cancels bookings, sends/receives messages.
- **Admin**: Moderates users, bookings, reviews, and messages.

### Data Stores
- **Users**: Stores user data (`user_id`, `email`, `password_hash`, `role`, etc.).
- **Properties**: Stores property data (`property_id`, `host_id`, `name`, `pricepernight`, etc.).
- **Bookings**: Stores booking data (`booking_id`, `property_id`, `user_id`, `start_date`, `end_date`, etc.).
- **Payments**: Stores payment data (`payment_id`, `booking_id`, `amount`, etc.).
- **Reviews**: Stores review data (`review_id`, `property_id`, `user_id`, `rating`, etc.).
- **Messages**: Stores message data (`message_id`, `sender_id`, `recipient_id`, `message_body`, etc.).

### Processes
1. **Register User**: Processes registration data, storing it in Users.
2. **Authenticate User**: Verifies login credentials against Users.
3. **Manage Properties**: Creates/updates property listings in Properties.
4. **Search Properties**: Retrieves property data for Guests.
5. **Create Booking**: Processes booking requests, storing in Bookings, using Properties for price calculation.
6. **Manage Bookings**: Updates booking statuses in Bookings.
7. **Process Payment**: Handles payment data, storing in Payments, with verification.
8. **Submit Review**: Processes review data, storing in Reviews.
9. **View Reviews**: Retrieves review data for display.
10. **Send Message**: Processes and stores messages in Messages.
11. **View Messages**: Retrieves message threads.
12. **Moderate Content**: Admin moderates reviews/messages in Reviews/Messages.

### Data Flows
- **Inputs**: Registration details, login credentials, booking dates, payment info, reviews, messages.
- **Outputs**: Property lists, booking confirmations, review lists, message threads.
- **Internal Flows**: Data between processes and data stores (e.g., storing bookings, retrieving user data).

