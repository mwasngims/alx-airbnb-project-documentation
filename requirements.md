
# Airbnb Clone – Backend Requirements Specification

**Repository**: `alx-airbnb-project-documentation`  
**File**: `requirements.md`

This document describes the functional and technical requirements for three critical backend features in the Airbnb Clone project.

---

## 1. 🔐 User Authentication

### Functional Requirements
- Register new users (host, guest).
- Authenticate existing users (login/logout).
- Manage user roles and permissions.
- Password hashing and secure storage.

### API Endpoints

| Method | Endpoint       | Description              |
|--------|----------------|--------------------------|
| POST   | /api/register  | Register new user        |
| POST   | /api/login     | Authenticate user        |
| POST   | /api/logout    | Invalidate session/token |

### Input Specifications

- **POST /api/register**
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "secure123",
  "role": "guest"
}
```

- **POST /api/login**
```json
{
  "email": "john@example.com",
  "password": "secure123"
}
```

### Output Example
```json
{
  "message": "Login successful",
  "token": "jwt.token.value"
}
```

### Validation Rules
- Email must be unique and valid.
- Password must be at least 8 characters.
- Role must be one of `guest`, `host`, `admin`.

### Performance Criteria
- Authentication should complete in < 300ms.
- Passwords hashed with bcrypt (or similar) at cost factor ≥ 12.

---

## 2. 🏡 Property Management

### Functional Requirements
- Hosts can create, update, and delete property listings.
- Guests can browse and view property details.
- Properties linked to the host’s user ID.

### API Endpoints

| Method | Endpoint            | Description                |
|--------|---------------------|----------------------------|
| POST   | /api/properties     | Create a new property      |
| GET    | /api/properties     | List all properties        |
| GET    | /api/properties/:id | Get property by ID         |
| PUT    | /api/properties/:id | Update property            |
| DELETE | /api/properties/:id | Delete property            |

### Input Specifications

- **POST /api/properties**
```json
{
  "host_id": "uuid-user-1",
  "name": "Seaside Villa",
  "description": "Beautiful 3-bedroom villa with ocean view.",
  "location": "Diani Beach",
  "pricepernight": 10000
}
```

### Output Example
```json
{
  "property_id": "uuid-prop-123",
  "message": "Property created successfully"
}
```

### Validation Rules
- Host ID must exist and have role `host`.
- Price must be a positive decimal.
- Required fields: name, description, location.

### Performance Criteria
- Listing retrieval in < 500ms with pagination (20 items/page).
- Property creation < 1s with indexed lookups.

---

## 3. 📅 Booking System

### Functional Requirements
- Guests can book available properties for a range of dates.
- System checks for date availability.
- Hosts and guests can view bookings.
- Cancel or update bookings based on status.

### API Endpoints

| Method | Endpoint            | Description                 |
|--------|---------------------|-----------------------------|
| POST   | /api/bookings       | Create a booking            |
| GET    | /api/bookings       | List all bookings           |
| GET    | /api/bookings/:id   | Get booking by ID           |
| PUT    | /api/bookings/:id   | Update booking status       |

### Input Specifications

- **POST /api/bookings**
```json
{
  "property_id": "uuid-prop-123",
  "user_id": "uuid-user-2",
  "start_date": "2025-07-01",
  "end_date": "2025-07-07"
}
```

### Output Example
```json
{
  "booking_id": "uuid-booking-987",
  "total_price": 60000,
  "status": "pending"
}
```

### Validation Rules
- Dates must not overlap with confirmed bookings.
- Total price = nights × pricepernight.
- Start date < end date.

### Performance Criteria
- Booking availability check < 1s.
- Bookings should be indexed by property and user.

---

## 📌 Future Enhancements
- Add rate-limiting on login.
- Support filtering properties by date, location, price.
- Add cancellation policies and refund logic.

---
