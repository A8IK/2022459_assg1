# API Documentation

## Base URL
- Development: `http://localhost:4000/api`
- Production: `https://api.your-domain.com/api`

## Authentication
All endpoints except `/auth/login` and `/auth/setup` require a valid JWT token in the Authorization header:

```
Authorization: Bearer <token>
```

## Endpoints

### Authentication

#### Sign In
```
POST /auth/login
Content-Type: application/json

{
  "email": "admin@example.com",
  "password": "password123"
}

Response (201):
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "admin": {
    "id": 1,
    "name": "Admin Name",
    "email": "admin@example.com"
  }
}
```

#### Initial Setup (Create First Admin)
```
POST /auth/setup
Content-Type: application/json

{
  "name": "Administrator",
  "email": "admin@example.com",
  "password": "securepassword"
}

Response (201):
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "admin": {
    "id": 1,
    "name": "Administrator",
    "email": "admin@example.com"
  }
}
```

### Tickets

#### Get All Tickets (with optional search)
```
GET /tickets?search=keyword
Authorization: Bearer <token>

Response (200):
{
  "tickets": [
    {
      "id": 1,
      "pnr": "VY4NLA",
      "bookingId": "TFBR00930285",
      "passengerName": "MR ABDULLA AL MAMUN",
      "airlineName": "Thai Airways",
      "airlineCode": "TG",
      "issueDateTime": "2026-05-05T09:18:00.000Z"
    }
  ]
}
```

#### Create Ticket
```
POST /tickets
Authorization: Bearer <token>
Content-Type: application/json

{
  "pnr": "VY4NLA",
  "bookingId": "TFBR00930285",
  "issueDateTime": "2026-05-05T09:18:00Z",
  "airlineName": "Thai Airways",
  "airlineCode": "TG",
  "passengerName": "MR ABDULLA AL MAMUN",
  "passengerType": "ADULT",
  "passportNo": "A01209877",
  "checkinBaggage": "23kg, 23kg",
  "cabinBaggage": "7 kg",
  "segments": [
    {
      "origin": "KUL",
      "destination": "BKK",
      "departureDate": "2026-05-15",
      "departureTime": "21:05",
      "duration": "2h 05m",
      "flightNumber": "TG - 416",
      "aircraftType": "Boeing 789",
      "departureAirport": "Kuala Lumpur International Airport",
      "arrivalAirport": "Suvarnabhumi Airport",
      "terminal": "Terminal: 1",
      "arrivalDateTime": "2026-05-15T22:10:00Z"
    }
  ],
  "notes": "Optional notes here"
}

Response (201):
{
  "ticket": {
    "id": 1,
    "pnr": "VY4NLA",
    ...
  }
}
```

#### Update Ticket
```
PUT /tickets/:id
Authorization: Bearer <token>
Content-Type: application/json

[Same payload as Create Ticket]

Response (200):
{
  "ticket": { ... }
}
```

#### Download PDF
```
GET /tickets/:id/pdf
Authorization: Bearer <token>

Response (200):
Returns PDF file as attachment
```

### Admin Management

#### Create New Admin
```
POST /admins
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "New Admin",
  "email": "admin2@example.com",
  "password": "securepassword"
}

Response (201):
{
  "admin": {
    "id": 2,
    "name": "New Admin",
    "email": "admin2@example.com",
    "created_at": "2026-05-11T10:30:00.000Z"
  }
}
```

### Health Check

#### Server Status
```
GET /health

Response (200):
{
  "status": "ok",
  "timestamp": "2026-05-11T10:30:00.000Z"
}
```

## Error Responses

### 400 - Bad Request
```json
{
  "error": "Invalid email format.",
  "statusCode": 400
}
```

### 401 - Unauthorized
```json
{
  "error": "Invalid credentials.",
  "statusCode": 401
}
```

### 404 - Not Found
```json
{
  "error": "Ticket not found.",
  "statusCode": 404
}
```

### 409 - Conflict
```json
{
  "error": "Admin already exists with this email.",
  "statusCode": 409
}
```

### 429 - Too Many Requests
```json
{
  "error": "Too many login attempts. Please try again after 15 minutes.",
  "statusCode": 429
}
```

### 500 - Server Error
```json
{
  "error": "Internal server error.",
  "statusCode": 500
}
```

## Rate Limiting

- **Login Endpoint**: 5 requests per 15 minutes per IP
- **Create Ticket**: 30 requests per minute per admin
- **General API**: 100 requests per minute per IP

## Data Validation

### Required Fields
- Ticket: pnr, bookingId, passengerName, passportNo, segments
- Admin: name (3-120 chars), email (valid format), password (6+ chars)

### Field Constraints
- PNR: 3-64 characters
- Booking ID: 3-128 characters
- Passenger Name: 3-180 characters
- Email: Valid email format, max 180 characters
- Password: Minimum 6 characters

## Response Headers

All successful responses include:
```
Content-Type: application/json
```

Authenticated endpoints verify:
```
Authorization: Bearer <JWT_Token>
```

## JWT Token Expiration

Tokens expire after 8 hours. Users must sign in again to get a new token.

## Example Client Usage

```javascript
// Login
const loginResponse = await fetch('http://localhost:4000/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ email: 'admin@example.com', password: 'password123' })
});
const { token } = await loginResponse.json();
localStorage.setItem('adminToken', token);

// Create Ticket
const ticketResponse = await fetch('http://localhost:4000/api/tickets', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  body: JSON.stringify(ticketData)
});
```

## Support & Debugging

For API issues:
1. Check server logs for error details
2. Verify Authorization token is included for protected endpoints
3. Ensure request Content-Type is `application/json`
4. Validate request payload against schema
5. Check rate limits if getting 429 errors
