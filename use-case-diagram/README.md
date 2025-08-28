# Airbnb Clone Backend Use Case Diagram

This section contains a use case diagram visualizing the interactions between users (actors) and the Airbnb Clone backend system, based on the defined features and functionalities.

## Files
- **README.md**: This file, describing the use case diagram and its purpose.
- **airbnb_clone_use_case.png**: A Draw.io diagram illustrating system interactions.

## Use Case Diagram Overview

The use case diagram captures interactions between **actors** (Guest, Host, Admin) and the Airbnb Clone backend system for key functionalities.

### Actors
- **Guest**: Books properties, makes payments, submits reviews, sends/receives messages.
- **Host**: Manages properties, confirms/cancels bookings, communicates with guests.
- **Admin**: Moderates users, bookings, reviews, and messages.
- **System**: The backend handling all functionalities.

### Use Cases
1. **User Authentication**:
   - Register: Sign up with email, password, role.
   - Log In: Authenticate with email and password.
   - Update Profile: Modify personal information.
   - Manage Users: Admin moderates user accounts.
2. **Property Management**:
   - Create Property Listing: Host adds a property.
   - Update Property Listing: Host modifies property details.
   - Search Properties: Guest searches/filters properties.
3. **Booking System**:
   - Create Booking: Guest books a property with start/end dates.
   - Manage Booking: Guest, Host, or Admin views/confirms/cancels bookings.
   - Calculate Total Price: System computes price dynamically.
4. **Payments**:
   - Make Payment: Guest pays using credit card, PayPal, or Stripe.
   - Verify Payment: System validates payment for booking confirmation.
5. **Reviews**:
   - Submit Review: Guest submits rating (1-5) and comment.
   - View Reviews: Guest views property reviews.
   - Moderate Reviews: Admin removes inappropriate reviews.
6. **Messaging**:
   - Send Message: Guest/Host sends messages.
   - View Message Threads: Guest/Host views conversation history.
   - Moderate Messages: Admin monitors messages.

### Relationships
- **Include**: Create Booking, Make Payment, Submit Review, and Send Message require Log In. Calculate Total Price is included in Create Booking; Verify Payment is included in Make Payment.
- **Extend**: Moderate Reviews extends View Reviews; Moderate Messages extends View Message Threads (Admin actions).
- **Associations**: Connect actors to their respective use cases (e.g., Guest to Create Booking, Host to Create Property Listing).




