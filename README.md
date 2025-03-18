<a href="https://www.wimplesolutions.com/" target="_blank" rel="noreferrer"> <img src="https://www.wimplesolutions.com/_next/image?url=%2Fwimplelogo2.png&w=3840&q=75" /> </a>

# Wimple Auto Transport API Documentation

## Authorize
#### Using curl</br>

```curl
curl -X POST https://www.wimplesolutions.com/your/api/endpoint \
-H "Content-Type: application/json" \
-H "x-api-key: YOUR_API_KEY_HERE" \
-d '{
    "key1": "value1",
    "key2": "value2"
}'
```

#### Using javascript</br>

```js
const apiKey = 'YOUR_API_KEY_HERE';
const url = 'https://www.wimplesolutions.com/your/api/endpoint';
const data = {
  key1: 'value1',
  key2: 'value2'
};

fetch(url, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-api-key': apiKey,
  },
  body: JSON.stringify(data)
})
.then(response => response.json())
```

## Booking API

This documentation provides details about the Booking API endpoints. Each endpoint is secured and requires a valid API key.


### Data Types: </br>
```
BookingDetails:
  booking_id (String): Unique booking identifier (min 10 max 18 char)
  email (String): Customer's email address (max 255 char)
  first_name (String): Customer's first names (max 20 char)
  last_name (String): Customer's last name (max 20 char)
  phone_number (String): Customer's phone number (max 20 char)
  business_name (String): Business name, if applicable (max 50 char)
  quote_date (DateTime): Date the quote was given
  booking_date (DateTime): Date the booking was made
  order_status (Int): Status of the order (1: Quote Given, 2: New Order, 3: Payment Approved)
  instructions (String): Special instructions (max 500 char)
  available_date (String): Available date for shipment (MM/DD/YYYY)
  promo_code (String): Applied promo code (max 255 char)
  payment_id (String): Payment ID (paypal or stripe payment) (max 255 char)
  quote_option (Int): Quote option selected (0: Cash Price, 1: Regular Price)
  created_at (DateTime): Current time
  pickup_location: {
    city (String): City for pickup (max 100 char)
    state (String): State for pickup (short name) (max 2 char)
    zip (String): Zip code for pickup (max 10 char)
    street_address (String): Street address for pickup (max 100 char)
    suite (String): Suite or apartment number (max 30 char)
    is_business (Int): Indicates if pickup is from a business (0: No, 1: Yes)
    first_name (String): First name of the recipient (max 20 char)
    last_name (String): Last name of the recipient (max 20 char)
    phone_number (String): Phone number of the recipient (max 20 char)
  }
  delivery_location: {
    city (String): City for delivery (max 100 char)
    state (String): State for delivery (short name) (max 2 char)
    zip (String): Zip code for delivery (max 10 char)
    street_address (String): Street address for delivery (max 100 char)
    suite (String): Suite or apartment number (max 30 char)
    is_business (Int): Indicates if delivery is to a business (0: No, 1: Yes)
    first_name (String): First name of the recipient (max 20 char)
    last_name (String): Last name of the recipient (max 20 char)
    phone_number (String): Phone number of the recipient (max 20 char)
  }
  pricing_details {
    distance (Float): Distance of the shipment (calculated distance between pickup location and delivery location)
    calculated_price (Float): Call the price calculator
    regular_price (Float): calculated_price * 1.20
    cash_price (Float): calculated_price * 1.17
    price_per_mile (Float): Call the price calculator
    confidence (Float): Call the price calculator
    trailer_type (Int): 1: Open, 2: Enclosed
    cash_price_discount (Float): Discount amount for cash price
    regular_price_discount (Float): Discount amount for regular price
    vehicles: (Array): {
      [
        VINValue (String): VIN value, if applicable (max 17 char) 
        is_inoperable (Boolean): true: Operable, false: Inoperable
        body (String): max 50 char
        make (String): max 50 char
        model (String): max 50 char
        year (String): max 4 char
      ]
    }
  }

```

### Endpoints

1. Create Booking
    #### Request: 
    - **URL:** https://www.wimplesolutions.com/api/v1/booking/create
    - **Method:** POST
    - **Headers:**
        * Content-Type: application/json
        * x-api-key: YOUR_API_KEY
    - **Body:**
      ```json
      {
        "booking_id": "ABCD123456",
        "email": "customer@example.com",
        "first_name": "John",
        "last_name": "Doe",
        "phone_number": "+1234567890",
        "business_name": "JD Enterprises",
        "quote_date": "2025-03-18T10:00:00Z",
        "booking_date": "2025-03-18T11:00:00Z",
        "order_status": 2,
        "instructions": "Handle with care.",
        "available_date": "03/20/2025",
        "promo_code": "SAVE10",
        "payment_id": "PAYPAL123456",
        "quote_option": 1,
        "created_at": "2025-03-18T12:00:00Z",
        "pickup_location": {
          "city": "Los Angeles",
          "state": "CA",
          "zip": "90001",
          "street_address": "123 Main St",
          "suite": "Apt 4B",
          "is_business": 1,
          "first_name": "Jane",
          "last_name": "Doe",
          "phone_number": "+1987654321"
        },
        "delivery_location": {
          "city": "New York",
          "state": "NY",
          "zip": "10001",
          "street_address": "456 Broadway Ave",
          "suite": "Suite 10",
          "is_business": 0,
          "first_name": "Mike",
          "last_name": "Smith",
          "phone_number": "+1123456789"
        },
        "pricing_details": {
          "distance": 2800.5,
          "calculated_price": 1500.0,
          "regular_price": 1800.0,
          "cash_price": 1755.0,
          "price_per_mile": 0.65,
          "confidence": 0.85,
          "trailer_type": 1,
          "cash_price_discount": 50.0,
          "regular_price_discount": 30.0,
          "vehicles": [
            {
              "VINValue": "1HGCM82633A123456",
              "is_inoperable": false,
              "body": "Sedan",
              "make": "Honda",
              "model": "Accord",
              "year": "2022"
            },
            {
              "VINValue": "2T3WFREV3DW123456",
              "is_inoperable": true,
              "body": "SUV",
              "make": "Toyota",
              "model": "RAV4",
              "year": "2020"
            }
          ]
        }
      }
      ```
      **Success Response**:
      - **Code**: 201
      - **Content**: Complete booking object

      **Error Response**:
      - **Code**: 500
        - **Content**: `{ "error": "Unable to create booking", "details": "Error message" }`
</br>

 2. Update Booking
    #### Request: 
    - **URL:** https://www.wimplesolutions.com/api/v1/booking/update?id=ABCD123456
    - **Method:** PUT
    - **Query Parameters**:
      - `id` (required): Booking ID
    - **Headers**:
        * Content-Type: application/json
        * x-api-key: YOUR_API_KEY
    - **Body**:
      ```json
      {
        "booking_id": "ABCD123456",
        "email": "customer@example.com",
        "first_name": "John",
        "last_name": "Doe",
        "phone_number": "+1234567890",
        "business_name": "JD Enterprises",
        "quote_date": "2025-03-18T10:00:00Z",
        "booking_date": "2025-03-18T11:00:00Z",
        "order_status": 2,
        "instructions": "Handle with care.",
        "available_date": "03/20/2025",
        "promo_code": "SAVE10",
        "payment_id": "PAYPAL123456",
        "quote_option": 1,
        "created_at": "2025-03-18T12:00:00Z",
        "pickup_location": {
          "city": "Los Angeles",
          "state": "CA",
          "zip": "90001",
          "street_address": "123 Main St",
          "suite": "Apt 4B",
          "is_business": 1,
          "first_name": "Jane",
          "last_name": "Doe",
          "phone_number": "+1987654321"
        },
        "delivery_location": {
          "city": "New York",
          "state": "NY",
          "zip": "10001",
          "street_address": "456 Broadway Ave",
          "suite": "Suite 10",
          "is_business": 0,
          "first_name": "Mike",
          "last_name": "Smith",
          "phone_number": "+1123456789"
        },
        "pricing_details": {
          "distance": 2800.5,
          "calculated_price": 1500.0,
          "regular_price": 1800.0,
          "cash_price": 1755.0,
          "price_per_mile": 0.65,
          "confidence": 0.85,
          "trailer_type": 1,
          "cash_price_discount": 50.0,
          "regular_price_discount": 30.0,
          "vehicles": [
            {
              "VINValue": "1HGCM82633A123456",
              "is_inoperable": false,
              "body": "sedan",
              "make": "Honda",
              "model": "Accord",
              "year": "2022"
            },
            {
              "VINValue": "2T3WFREV3DW123456",
              "is_inoperable": true,
              "body": "suv",
              "make": "Toyota",
              "model": "RAV4",
              "year": "2020"
            }
          ]
        }
      }
      ```
      **Success Response**:
      - **Code**: 200
      - **Content**: `{ "message": "BookingDetails updated successfully.", "data": {...} }`

      **Error Responses**:
      - **Code**: 400
        - **Content**: `{ "error": "BookingDetails ID is required." }`
      - **Code**: 500
        - **Content**: `{ "error": "An error occurred while updating the BookingDetails." }`

</br>

3. Calculate Price
    #### Request: 
    - **URL:** https://www.wimplesolutions.com/api/v1/price-calculator/post
    - **Method:** POST
    - **Headers:**
        * Content-Type: application/json
        * x-api-key: YOUR_API_KEY
    - **Body:**
      ```json
      {
        "pickup": {
          "city": "Los Angeles",
          "state": "CA",
          "zip": "90001"
        },
        "delivery": {
          "city": "Dallas", 
          "state": "TX",
          "zip": "75001"
        },
        "vehicles": [
          {
            "year": "2020",
            "make": "Ford",
            "model": "F-150",
            "type": "sedan",
            "is_inoperable": false
          }
        ],
        "trailer_type": "open"
      }
      ```
      **Success Response**:
      - **Code**: 200
      - **Content**:
      ```json
      {
          "meta": {
              "status": "success"
          },
          "data": {
              "price": 750,
              "price_per_mile": 0.52,
              "confidence": 86
          }
      }
      ```

      **Error Responses**:
      - **Code**: 400
        - **Content**: `{ "error": "Bad request - Invalid input parameters" }`
      - **Code**: 401
        - **Content**: `{ "error": "Unauthorized - Invalid API key" }`
      - **Code**: 422
        - **Content**: `{ "error": "Unprocessable Entity - Invalid data format" }`
      - **Code**: 429
        - **Content**: `{ "error": "Too Many Requests - Rate limit exceeded" }`
      - **Code**: 500
        - **Content**: `{ "error": "Failed to calculate price" }`


## Error Codes

| Status Code | Meaning                  | Description                                           |
|-------------|--------------------------|-------------------------------------------------------|
| 200         | OK                       | The request was successful                            |
| 201         | Created                  | The resource was successfully created                 |
| 400         | Bad Request              | The request was invalid or cannot be processed        |
| 401         | Unauthorized             | Authentication failed or not provided                 |
| 403         | Forbidden                | Authentication succeeded but permission was denied    |
| 404         | Not Found                | The requested resource was not found                  |
| 405         | Method Not Allowed       | The HTTP method is not supported for this endpoint    |
| 422         | Unprocessable Entity     | The request was well-formed but has semantic errors   |
| 429         | Too Many Requests        | Rate limit has been exceeded                          |
| 500         | Internal Server Error    | An unexpected error occurred on the server            |
