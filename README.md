# API Documentation

Base URL: `https://api.wimplesolutions.com`

## Authentication Endpoints

### 1. User Login
**POST** `/v1/auth/login`

Authenticate a user and receive a JWT token.

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "username": "string",
  "password": "string"
}
```

**Response:**
```json
{
  "token": "string",
  "tokenExpiryDate": "2024-12-31T23:59:59Z",
  "roles": ["PARTNER"]
}
```

**Status Codes:**
- `200 OK` - Login successful
- `401 Unauthorized` - Invalid credentials
- `400 Bad Request` - Invalid input format
- `500 Internal Server Error` - Server error

---

### 2. User Logout
**POST** `/v1/auth/logout`

Logout the current user and blacklist the JWT token.

**Headers:**
```
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "message": "Log out successful"
}
```

**Status Codes:**
- `200 OK` - Logout successful
- `401 Unauthorized` - User not authenticated
- `404 Not Found` - User not found
- `500 Internal Server Error` - Server error

---

### 3. Get Current User
**GET** `/v1/auth/me`

Get the current authenticated user's profile information.

**Headers:**
```
Authorization: Bearer <jwt_token>
```

**Response:**
```json
{
  "id": "integer",
  "username": "string",
  "email": "string",
  "roles": ["PARTNER"]
}
```

**Status Codes:**
- `200 OK` - Profile retrieved successfully
- `401 Unauthorized` - User not authenticated
- `404 Not Found` - User not found
- `500 Internal Server Error` - Server error

---

## Price Calculator Endpoint

### 4. Calculate Recommended Price
**POST** `/v1/price-calculator`

Calculate recommended shipping price based on pickup/delivery locations and vehicle details.

**Headers:**
```
Content-Type: application/json
Authorization: Bearer <jwt_token>
```

**Required Roles:** `PARTNER`

**Request Body:**
```json
{
  "pickup": {
    "city": "string",
    "state": "string",
    "zip": "string"
  },
  "delivery": {
    "city": "string",
    "state": "string",
    "zip": "string"
  },
  "trailer_type": "open | enclosed",
  "vehicles": [
    {
      "year": "integer",
      "make": "string",
      "model": "string",
      "type": "sedan | suv | van | coupe_2_doors | pickup_2_doors | pickup_4_doors",
      "is_inoperable": "boolean"
    }
  ]
}
```

**Example Request:**
```json
{
  "pickup": {
    "city": "Los Angeles",
    "state": "CA",
    "zip": "90210"
  },
  "delivery": {
    "city": "New York",
    "state": "NY",
    "zip": "10001"
  },
  "trailer_type": "open",
  "vehicles": [
    {
      "year": 2020,
      "make": "Toyota",
      "model": "Camry",
      "type": "sedan",
      "is_inoperable": false
    }
  ]
}
```

**Response:**
```json
{
  "calculated_price": 1250
}
```

**Status Codes:**
- `200 OK` - Price calculated successfully
- `400 Bad Request` - Invalid input parameters
- `401 Unauthorized` - Invalid or missing authentication
- `403 Forbidden` - Insufficient permissions
- `422 Unprocessable Entity` - Invalid data format
- `429 Too Many Requests` - Rate limit exceeded
- `500 Internal Server Error` - Server error

---

## Authentication

All protected endpoints require a valid JWT token in the Authorization header:

```
Authorization: Bearer <your_jwt_token>
```

Tokens are obtained through the login endpoint and have an expiration time included in the login response.

## Error Responses

All endpoints return error responses in the following format:

```json
{
  "error": "Error description",
  "message": "Detailed error message"
}
```

## Notes

- All timestamps are in ISO 8601 format
- Prices are returned as integers (cents) or rounded to 2 decimal places
- The `trailer_type` field accepts either "open" or "enclosed"
- Vehicle `type` **must be one of**: "sedan", "suv", "van", "coupe_2_doors", "pickup_2_doors", "pickup_4_doors"
- Different pricing tiers are applied based on user roles (PARTNER vs ADMIN)