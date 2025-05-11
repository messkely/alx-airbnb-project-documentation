# 🏠✨Backend Requirement Specifications for Airbnb Clone Application

This document provides an API overview of the key features and functionalities of the Airbnb platform. It outlines core components such as user authentication, property listing, booking management, messaging, reviews, and payment processing.

---

## 1. User Authentication & Authorization

### Features
- User Registration
- Login/Logout
- Email Verification
- Password Reset
- Token-based Authentication (JWT)

### API Endpoints

| Method | Endpoint                  | Description                     |
|--------|---------------------------|---------------------------------|
| POST   | /api/auth/register        | Register new user               |
| POST   | /api/auth/login           | Login user                      |
| POST   | /api/auth/logout          | Logout user                     |
| POST   | /api/auth/verify          | Email verification              |
| POST   | /api/auth/reset           | Request password reset          |
| POST   | /api/auth/reset/confirm   | Reset password with token       |

### Input/Output Example

**POST /api/auth/register**
```json
{
  "email": "kamal@example.com",
  "password": "S3cureP@ss!",
  "name": "Kamal Esskely"
}
```

**Response**
```json
{
  "message": "User created. Please verify your email."
}
```

### Validation Rules
- Email must be unique and valid format
- Password: min 8 characters, at least 1 uppercase, 1 number, 1 special character
- Rate limit login attempts: 5 tries per 10 minutes

### Performance Criteria
- Auth routes respond < 300ms
- Token expiration: 1h access / 7d refresh

---

## 2. Property Management

### Features
- Hosts can create, update, delete properties
- Attach photos, pricing, availability
- View own listings

### API Endpoints

| Method | Endpoint                     | Description                |
|--------|------------------------------|----------------------------|
| POST   | /api/properties              | Create new property        |
| GET    | /api/properties              | List all properties        |
| GET    | /api/properties/:id          | Get property by ID         |
| PUT    | /api/properties/:id          | Update property            |
| DELETE | /api/properties/:id          | Delete property            |
| POST   | /api/properties/:id/photos   | Upload photos              |

### Input/Output Example

**POST /api/properties**
```json
{
  "title": "Modern Loft in Cairo",
  "location": "Cairo, Egypt",
  "price": 75,
  "description": "Beautiful view, modern amenities",
  "amenities": ["WiFi", "Air Conditioning"],
  "availability": { "start": "2025-05-01", "end": "2025-07-01" }
}
```

**Response**
```json
{
  "id": "property_123",
  "message": "Property listed successfully"
}
```

### Validation Rules
- Title: required, max 100 chars
- Price: positive number
- Availability dates: start < end
- Max 10 photos per property

### Performance Criteria
- Listing retrieval < 500ms
- Store images via CDN or S3

---

## 3. Booking System

### Features
- Search by date/location
- Book available listings
- Prevent double-bookings
- View & cancel bookings

### API Endpoints

| Method | Endpoint               | Description                |
|--------|------------------------|----------------------------|
| POST   | /api/bookings          | Create booking             |
| GET    | /api/bookings          | List user bookings         |
| GET    | /api/bookings/:id      | Get booking details        |
| DELETE | /api/bookings/:id      | Cancel booking             |
| POST   | /api/bookings/check    | Check availability         |

### Input/Output Example

**POST /api/bookings**
```json
{
  "property_id": "property_123",
  "check_in": "2025-06-10",
  "check_out": "2025-06-15",
  "guests": 2
}
```

**Response**
```json
{
  "booking_id": "booking_456",
  "status": "confirmed",
  "total_price": 375
}
```

### Validation Rules
- Dates must not overlap existing bookings
- Stay must be 1–30 nights
- Guests ≤ property's max allowed

### Performance Criteria
- Availability check < 300ms
- Use DB transactions to prevent race conditions

---
