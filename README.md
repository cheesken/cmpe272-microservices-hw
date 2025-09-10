# Microservices Assignment 09/10/2025

Microservices architecture is a design approach where a single application is composed of small, independent services that communicate over well-defined APIs. Each service is focused on a specific business capability and can be developed, deployed, and scaled
independently.

This assignment illustrates microservices architecture using Python and Flask.

I have created two microservices: one for managing users and another for managing orders. These services will communicate with each other via HTTP requests

## Microservices
### User Service (user_service.py)
- Port: 5001
- Endpoints:
  * GET /users/<user_id> - Retrieve user information
  * POST /users - Create a new user
- Sample Data: Pre-loaded with Alice and Bob

### Order Service (order_service.py)
- Port: 5002
- Endpoints:
  * GET /orders/<order_id> - Retrieve order information (includes user details)
  * POST /orders - Create a new order
- Sample Data: Pre-loaded with orders for Laptop and Smartphone

### How it works?
1. User Service:
- Runs on port 5001.
- Provides endpoints to create and retrieve users.
- Stores user data in a simple in-memory dictionary.

2. Order Service:
- Runs on port 5002.
- Provides endpoints to create and retrieve orders.
- When retrieving an order, it makes an HTTP request to the User Service to fetch the associated user details.

## Setup / Installation
### Prerequisites
- Python 3.11+
- pip package manager

### Install Dependencies
```pip install flask requests```

### Running
1. Start User Service
```python user_service.py```
The User Service will start on http://localhost:5001

2. Start Order Service (in a new terminal)
```python order_service.py```
The Order Service will start on http://localhost:5002

Note: Both services must be running simultaneously because the Order Service depends on the User Service.

## API Testing
### Get a user:
```curl http://localhost:5001/users/1```\
Response:
```
{
  "name": "Alice",
  "email": "alice@example.com"
}
```

### Create a new user:
```
curl -X POST http://localhost:5001/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Charlie", "email": "charlie@example.com"}'
```
Response:
```
{
  "user_id": 3
}
```

### Get an order:
Note:  This includes user details.
```curl http://localhost:5002/orders/1```\
Response:
```
{
  "user_id": 1,
  "product": "Laptop",
  "user": {
    "name": "Alice",
    "email": "alice@example.com"
  }
}
```

### Create a new order:
```
curl -X POST http://localhost:5002/orders \
  -H "Content-Type: application/json" \
  -d '{"user_id": 2, "product": "Tablet"}'
```
Response:
```
{
  "order_id": 3
}
```
