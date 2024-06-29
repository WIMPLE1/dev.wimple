# Wimple Auto Transport API Documentation

<a href="https://www.wimplesolutions.com/" target="_blank" rel="noreferrer"> <img src="https://www.wimplesolutions.com/_next/image?url=%2Fwimplelogo2.png&w=3840&q=75" /> </a>

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

* Booking Details:
    * id (Int): Auto-incremented booking ID
    * booking_id (String): Unique booking identifier (min 10 max 30 char)
    * email (String): Customer's email address (max 255 char)
    * first_name (String): Customer's first names (max 20 char)
    * last_name (String): Customer's last name (max 20 char)
    * phone_number (String): Customer's phone number (max 20 char)
    * business_name (String): Business name, if applicable (max 50 char)
    * quote_date (String): Date the quote was given (MM/DD/YYYY)
    * book_date (String): Date the booking was made (MM/DD/YYYY)
    * order_status (Int): Status of the order (0: Quote Given, 1: New Order, 2: Payment Approved)
    * ship_from (String): Origin address (max 60 char)
    * ship_to (String): Destination address (max 60 char)
    * distance (Int): Distance of the shipment (calculated distance between ship_from and ship_to)
    * price (Int): Price of the shipment (call price-calculator api)
    * instructions (String): Special instructions (max 500 char)
    * available_date (String): Available date for shipment (MM/DD/YYYY)
    * promo_code (String): Applied promo code (max 255 char)
    * payment_id (String): Payment ID (paypal or stripe payment) (max 255 char)
    * quote_option (Int): Quote option selected (0: Discounted Price, 1: Express Price)
    * vehicle_id (Int): Unique vehicle ID (relation with vehicle details id)
    * delivery_id (Int): Unique delivery ID (relation with delivery details id)
    * pickup_id (Int): Unique pickup ID (relation with pickup details id)

<br>

* Vehicle Details:
    * id (Int): Auto-incremented vehicle details ID
    * year (String): Year of the vehicle, multiple years can be joined by | (max 50 char)
    * make (String): Make of the vehicle, multiple makes can be joined by | (max 140 char)
    * model (String): Model of the vehicle, multiple models can be joined by | (max 410 char)
    * body (String): Body type of the vehicle, multiple body types can be joined by | (max 370 char)
    * vehicle_condition (String): Condition of the vehicle, multiple conditions can be joined by | (1: Running, 2: Non-Running) (max 20 char)
    * transport_type (Int): Transport type (1: Open, 2: Enclosed)

<br>

* Delivery Details:
    * id (Int): Auto-incremented delivery details ID
    * street_address (String): Street address for delivery (max 100 char)
    * suite (String): Suite or apartment number (max 30 char)
    * city (String): City for delivery (max 28 char)
    * state (String): State for delivery (max 15 char)
    * zip_code (String): Zip code for delivery (max 5 char)
    * is_business (Int): Indicates if delivery is to a business (0: No, 1: Yes)
    * full_name (String): Full name of the recipient (max 40 char)
    * phone_number (String): Phone number of the recipient (max 20 char)

<br>

* Pickup Details:
    * id (Int): Auto-incremented pickup details ID
    * street_address (String): Street address for pickup (max 100 char)
    * suite (String): Suite or apartment number (max 30 char)
    * city (String): City for pickup (max 28 char)
    * state (String): State for pickup (max 15 char)
    * zip_code (String): Zip code for pickup (max 5 char)
    * is_business (Int): Indicates if pickup is from a business (0: No, 1: Yes)
    * full_name (String): Full name of the sender (max 40 char)
    * phone_number (String): Phone number of the sender (max 20 char)


### Endpoints

1. Add Booking
    #### Request: 
    * URL: https://www.wimplesolutions.com/api/booking/add
    * Method: POST
    * Headers:
        * Content-Type: application/json
        * x-api-key: YOUR_API_KEY
    * Body:
      ```
      {
        "booking_id": "123456789",
        "email": "johndoe@gmail.com",
        "first_name": "John",
        "last_name": "Doe",
        "phone_number": "",
        "business_name": "",
        "quote_date": "06/27/2024",
        "book_date": "",
        "order_status": 0,
        "ship_from": "State, ST, ZIP",
        "ship_to": "State, ST, ZIP",
        "distance": 100,
        "price": 1000,
        "instructions": "",
        "available_date": "07/30/2024",
        "promo_code": "",
        "payment_id": "",
        "quote_option": 0,
        "vehicle_details": {
            "year": "1998|2020|2021|2014",
            "make": "Toyota|Honda|Ford|BMW",
            "model": "Camry|Accord|F-150|X5",
            "body": "Sedan|Sedan|Truck|SUV",
            "vehicle_condition": "1|2|2|1",
            "transport_type": 1
        },
        "delivery_details": {
            "street_address": "",
            "suite": "",
            "city": "",
            "state": "",
            "zip_code": "",
            "is_business": 0,
            "full_name": "",
            "phone_number": ""
        },
        "pickup_details": {
            "street_address": "",
            "suite": "",
            "city": "",
            "state": "",
            "zip_code": "",
            "is_business": 0,
            "full_name": "",
            "phone_number": ""
        }
      }
      ```
    #### Response:
    * Status: 201 OK
    * Body: 
      ```
      {
        <booking data>
      }
      ```
    #### Error Response:
    * Status: 405 Bad Request
    * Body: 
      ```
      {
        "error": "Method not allowed"
      }
      ```
    * Status: 500 Unable to create booking
    * Body: 
      ```
      {
        "error": "Unable to create booking"
      }
      ```

</br>

2. Update Booking
    #### Request: 
    * URL: https://www.wimplesolutions.com/api/booking/update
    * Method: PUT
    * Headers:
        * Content-Type: application/json
        * x-api-key: YOUR_API_KEY
    * Body:
      ```
      {
        "id": 1,
        "booking_id": "123456789",
        "email": "johndoe@gmail.com",
        "first_name": "John",
        "last_name": "Doe",
        "phone_number": "123-456-7890",
        "business_name": "Doe Enterprises",
        "quote_date": "06/27/2024",
        "book_date": "06/27/2024",
        "order_status": 1,
        "ship_from": "State, ST, ZIP",
        "ship_to": "State, ST, ZIP",
        "distance": 100,
        "price": 1596,
        "instructions": "Handle with care.",
        "available_date": "07/30/2024",
        "promo_code": "SAVE20",
        "payment_id": "PAY123456",
        "quote_option": 0,
        "vehicle_id": 1,
        "delivery_id": 1,
        "pickup_id": 1,
        "vehicle_details": {
            "year": "1998|2020|2021|2014",
            "make": "Toyota|Honda|Ford|BMW",
            "model": "Camry|Accord|F-150|X5",
            "body": "Sedan|Sedan|Truck|SUV",
            "vehicle_condition": "1|2|2|1",
            "transport_type": 1
        },
        "delivery_details": {
            "street_address": "789 Destination Ave",
            "suite": "Suite 101",
            "city": "Destination City",
            "state": "Destination",
            "zip_code": "67890",
            "is_business": 0,
            "full_name": "John Doe",
            "phone_number": "098-765-4321"
        },
        "pickup_details": {
            "street_address": "123 Origin St",
            "suite": "Suite 202",
            "city": "Origin City",
            "state": "Origin",
            "zip_code": "12345",
            "is_business": 1,
            "full_name": "John Doe",
            "phone_number": "123-456-7890"
        }
      }
      ```
    #### Response:
    * Status: 201 OK
    * Body: 
      ```
      {
        <booking data>
      }
      ```
    #### Error Response:
    * Status: 405 Bad Request
    * Body: 
      ```
      {
        "error": "Method not allowed"
      }
      ```
    * Status: 500 Unable to update booking
    * Body: 
      ```
      {
        "error": "Unable to update booking"
      }
      ```

</br>

3. Calculate Price
    #### Request: 
    * URL: https://www.wimplesolutions.com/api/price-calculator/get
    * Method: POST
    * Headers:
        * Content-Type: application/json
        * x-api-key: YOUR_API_KEY
    * Body:
      ```
      {
        "distance": 100,
        "transport_type": 1,
        "vehicle_conditions": "2",
        "ship_from_state": "NY",
        "ship_to_state": "TX",
        "body_types": "Sedan"
      }
      ```
    #### Response:
    * Status: 200 OK
    * Body: 
      ```
      {
        "price": 599
      }
      ```
    #### Error Response:
    * Status: 400 Bad Request
    * Body: 
      ```
      {
        "error": "Missing required fields"
      }
      ```
    * Status: 405 Method not allowed
    * Body: 
      ```
      {
        "error": "Method not allowed"
      }
      ```
    * Status: 500 Internal Server Error
    * Body: 
      ```
      {
        "error": "Internal server error"
      }
      ```
4. Multi car Calculate Price
    #### Request: 
    * URL: https://www.wimplesolutions.com/api/price-calculator/get
    * Method: POST
    * Headers:
        * Content-Type: application/json
        * x-api-key: YOUR_API_KEY
    * Body:
      ```
      {
        "distance": 100,
        "transport_type": 1,
        "vehicle_conditions": "1|2|1|1",
        "ship_from_state": "NY",
        "ship_to_state": "TX",
        "body_types": "Sedan|Sedan|Truck|SUV"
      }
      ```
    #### Response:
    * Status: 200 OK
    * Body: 
      ```
      {
        "price": 1596
      }
      ```
    #### Error Response:
    * Status: 400 Bad Request
    * Body: 
      ```
      {
        "error": "Missing required fields"
      }
      ```
    * Status: 405 Method not allowed
    * Body: 
      ```
      {
        "error": "Method not allowed"
      }
      ```
    * Status: 500 Internal Server Error
    * Body: 
      ```
      {
        "error": "Internal server error"
      }
      ```

