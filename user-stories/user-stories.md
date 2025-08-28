# User Stories

The following user stories cover the core functionalities of the Airbnb Clone backend, including user authentication, property management, booking, payments, reviews, and messaging.

1. **User Registration**
   - **As a user**, I want to register an account with my email, password, and personal details **so that** I can access the platform as a guest, host, or admin.
   - **Details**: Users sign up with a unique email, hashed password, and role (guest, host, admin) to perform role-specific actions.

2. **Property Listing Creation**
   - **As a host**, I want to create a property listing with details like name, description, location, and price per night **so that** guests can discover and book my property.
   - **Details**: Hosts add properties to the platform, specifying key details to attract guests.

3. **Booking Creation**
   - **As a guest**, I want to book a property by specifying start and end dates **so that** I can secure a place to stay for my trip.
   - **Details**: Guests create bookings after logging in, with the total price calculated dynamically.

4. **Payment Processing**
   - **As a guest**, I want to make a payment for a booking using my preferred method (credit card, PayPal, or Stripe) **so that** I can confirm my reservation.
   - **Details**: Guests process payments, which are verified by the system to confirm bookings.

5. **Review Submission**
   - **As a guest**, I want to submit a review with a rating and comment for a property I’ve stayed at **so that** I can share my experience with other users.
   - **Details**: Guests provide feedback (rating 1-5 and comment) after completing a booking.

6. **Message Communication**
   - **As a guest**, I want to send a message to a host to inquire about a property **so that** I can clarify details before booking.
   - **Details**: Guests and hosts communicate via messages, with threads stored for reference.

7. **Booking Management**
   - **As a host**, I want to manage bookings for my properties, including confirming or canceling them, **so that** I can control my property’s availability.
   - **Details**: Hosts confirm or cancel bookings, complementing guest and admin management actions.

8. **Admin Moderation**
   - **As an admin**, I want to moderate reviews and messages **so that** I can ensure content complies with platform policies.
   - **Details**: Admins review and remove inappropriate reviews and messages, extending from viewing actions.

