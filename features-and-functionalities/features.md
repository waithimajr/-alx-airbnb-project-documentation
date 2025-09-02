# Features & Functionalities — Airbnb Clone (Backend)

## Overview
This document lists the core features and supporting functionality the backend must provide to implement an Airbnb-like service. Grouped by domain area, each feature includes brief acceptance notes.


## 1. Authentication & Authorization
- **User registration**: Create account with email, password (bcrypt), name, phone. Email uniqueness enforced.
- **Login (session/JWT)**: Issue JWT on successful login; support access token & refresh token lifecycle.
- **Password reset**: Secure reset via email token with expiry.
- **Roles & permissions**: `guest`, `host`, `admin`. Role-based access control (RBAC) for endpoints.
- **Account verification**: Email verification flow (optional).

**Acceptance notes:** Endpoints: `POST /auth/register`, `POST /auth/login`, `POST /auth/refresh`, `POST /auth/reset`, middleware to verify JWT and roles.


## 2. User Management
- **Profile CRUD**: Read/update user profile; view other public profiles (hosts).
- **Host onboarding**: Extra profile fields for hosts (company, bio, verified_id).
- **Account deletion**: Soft-delete users, optional data retention policy.

**Acceptance notes:** `GET /users/:id`, `PUT /users/:id` (auth), `DELETE /users/:id` (auth+admin).


## 3. Property Management
- **Property CRUD**: Hosts can create, update, delete property listings.
- **Property attributes**: title, description, location (city, coordinates), amenities, photos, price_per_night, max_guests, rules, availability calendar.
- **Photo upload**: Accept image uploads (S3 or other object store) with URLs saved in DB.
- **Amenities & categories**: Reusable amenities table and property_amenities relation.
- **Search & filtering**: Query by location, price range, dates availability, guest count, amenities, rating.
- **Property moderation**: Admin review / approve listings.

**Acceptance notes:** `GET /properties`, `POST /properties`, `GET /properties/:id`, `PUT /properties/:id`, `DELETE /properties/:id`. Support paginated responses.

## 4. Booking System
- **Create booking**: Guests can request or instantly book depending on property settings.
- **Availability check**: Prevent double-booking using transactions/row locks; validate start/end dates and min/max stay.
- **Booking lifecycle**: statuses: `pending`, `confirmed`, `cancelled`, `completed`.
- **Host approval / auto-confirmation**: Configurable per property.
- **Pricing & fees**: Total price calculation (nights × price_per_night + cleaning + service fee + taxes).
- **Cancellation policies**: Enforce refund rules and status transitions.
- **Calendar integration**: Return availability for calendar UI.

**Acceptance notes:** `POST /bookings`, `GET /bookings/:id`, `GET /users/:id/bookings`, ensure ACID when creating bookings.


## 5. Payments & Payouts
- **Payment processing**: Integrate Stripe/PayPal for charges. Tokenize card details on frontend.
- **Charge & capture**: Charge guest and hold/transfer to host per business logic.
- **Refunds**: Support full/partial refunds based on cancellation rules.
- **Payouts to hosts**: Schedule payouts (Stripe Connect or similar).
- **Payment records**: Store transaction id, status, amount, fees, method.

**Acceptance notes:** `POST /payments/charge`, webhooks endpoint to receive payment events, secure webhook verification.


## 6. Reviews & Ratings
- **Create review**: Guests can leave rating (1–5) and comment after stay.
- **Review moderation**: Hosts can respond; admin tools to remove abusive reviews.
- **Aggregate ratings**: Compute property average rating for search/sorting.

**Acceptance notes:** `POST /properties/:id/reviews`, `GET /properties/:id/reviews`.


## 7. Messaging & Notifications
- **In-app messaging**: Conversations between guest and host (threaded messages).
- **Email notifications**: Booking confirmations, payment receipts, password reset, host messages.
- **Push/web notifications**: Optional real-time updates (WebSockets or push).

**Acceptance notes:** `POST /conversations`, `POST /messages`, notification worker/process.


## 8. Admin & Moderation
- **User/property moderation**: View and manage flagged content, suspend users.
- **Reports & analytics**: Basic metrics (bookings per period, revenue totals, active listings).
- **Audit logs**: Track critical actions (deletes, role changes, payments).

**Acceptance notes:** Admin-only endpoints protected by RBAC.


## 9. Operational Concerns
- **Rate limiting & abuse protection**: Prevent brute force & spam.
- **Logging & monitoring**: Structured logs, metrics, and tracing for performance (Prometheus/ELK).
- **Backups & DR**: Regular DB backups and recovery plan.
- **Security best practices**: Input validation, prepared statements, secure storage of secrets.


## 10. API & Developer Experience
- **RESTful endpoints** with consistent patterns and pagination.
- **OpenAPI / Swagger** spec for endpoints and models.
- **Validation & error handling**: Clear error codes and messages.
- **Environment configuration**: 12-factor app readiness.


## File links / artifacts
- Draw.io diagram: `features-and-functionalities/airbnb_features.drawio` (editable)
- PNG export for reviewers: `features-and-functionalities/airbnb_features.png`
