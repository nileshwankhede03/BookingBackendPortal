# Booking System — MultiGenesys Assignment
Spring Boot 3 + Java 17 booking system with JWT auth and RBAC.

## Features
- Resources: CRUD for ADMIN; read-only for USER
- Reservations: create, read, update, delete; filtering (status, minPrice, maxPrice), pagination, sorting
- JWT-based stateless authentication
- Roles: ADMIN, USER
- Seed users included (admin/admin123, user/user123)

## Tech
- Java 17, Spring Boot 3.x, Spring Security, Spring Data JPA
- MySQL as RDBMS
- Maven

## Prerequisites
- Java 17+
- Maven
- MySQL running

#BookingBackendPortal 

Project Purpose 
BookingBackendPortal is a robust backend service to manage user 
bookings with secure authentication, role-based access control (RBAC), 
and RESTful APIs. It is optimized for correctness, security, and 
performance, supporting features like filtering, pagination, and 
comprehensive error handling. 

Features 
• Robust RBAC: Users (Admin, User) have distinct permissions for bookings and user management.
• JWT-based authentication: Secure login sessions for all endpoints.
• Strong password hashing: Passwords are hashed using bcrypt before storage.
• RESTful APIs: CRUD endpoints for bookings with validation and standardized responses.
• Filtering & Pagination: List endpoints allow querying and paginated responses.
• Modular, readable codebase for maintenance and extension.
• Clear, actionable error responses and validation feedback.

Installation & Setup 
Clone repository and navigate:
git clone https://github.com/nileshwankhede03/BookingBackendPortal.git

API Usage 
Authentication :
A) POST /api/auth/register
• Payload: { "username": "string", "password": "string", "role": "user|admin" }
• Response: User created message, JWT token

B) POST /api/auth/login 
• Payload: { "username": "string", "password": "string" }
• Response: JWT token.

Booking CRUD Endpoints (Protected with JWT :
GET /api/bookings?status=pending&page=1&limit=10 

RBAC: User sees own bookings, admin can see all 
Filtering: status, date, user, etc. 
Pagination: page, limit query params 

POST /api/bookings
Create a new booking (RBAC: user/admin) 
Body: Booking details 

PUT /api/bookings/:id
Update booking by ID (RBAC: owner or admin) 

DELETE /api/bookings/:id
Delete booking (RBAC: owner or admin) 

User Management (Admins only) 
GET /api/users - List users (admin) 
PUT /api/users/:id/role - Change user role (admin) 

Security & Validation 
• JWT tokens are required for all protected endpoints (sent via Authorization: Bearer 
header). 
• Passwords are hashed with bcrypt before storing in the database. 
• All sensitive actions are RBAC-checked; admins have exclusive endpoints. 
• Input data is validated (no SQL injection, XSS, or unsafe entries). 

Filtering & Pagination 
List endpoints accept filter (e.g., status, user, date) and pagination (page, limit). 
Responses include pagination metadata: total, current page, pages, next/prev. 
Error Handling 
• All endpoints return clear, semantic error responses: 
• 401 Unauthorized, 403 Forbidden, 404 Not Found, 400 Validation errors, 500 Internal 
server error.

🌀 comprehensive Postman collection with all the API endpoints, JSON examples, and authentication setup.

Postman Collection Setup

1. Environment Variables

Create a Postman environment with these variables:

· baseUrl: http://localhost:8080
· jwtToken: (will be set after login)

2. Authentication Endpoints

POST /auth/login

```http
POST {{baseUrl}}/auth/login
Content-Type: application/json
```

Request Body:

```json
{
    "username": "admin",
    "password": "admin123"
}
```

Response:

```json
{
    "token": "eyJhbGciOiJIUzUxMiJ9...",
    "type": "Bearer",
    "id": 1,
    "username": "admin",
    "roles": ["ROLE_ADMIN", "ROLE_USER"]
}
```

Tests Tab (to automatically set token):

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

const response = pm.response.json();
pm.environment.set("jwtToken", response.token);
```

3. Resource Endpoints

GET /resources (Public - No auth required)

```http
GET {{baseUrl}}/resources?page=0&size=10&sortBy=name&sortDir=asc
```

GET /resources/{id} (Public)

```http
GET {{baseUrl}}/resources/1
```

POST /resources (Admin only)

```http
POST {{baseUrl}}/resources
Content-Type: application/json
Authorization: Bearer {{jwtToken}}
```

Request Body:

```json
{
    "name": "Executive Suite",
    "type": "ROOM",
    "description": "Luxury executive meeting room",
    "capacity": 8
}
```

PUT /resources/{id} (Admin only)

```http
PUT {{baseUrl}}/resources/1
Content-Type: application/json
Authorization: Bearer {{jwtToken}}
```

Request Body:

```json
{
    "name": "Updated Conference Room",
    "type": "ROOM",
    "description": "Updated description",
    "capacity": 25
}
```

DELETE /resources/{id} (Admin only)

```http
DELETE {{baseUrl}}/resources/1
Authorization: Bearer {{jwtToken}}
```

4. Reservation Endpoints

GET /reservations (With filters)

```http
GET {{baseUrl}}/reservations?status=CONFIRMED&minPrice=50&maxPrice=500&page=0&size=10&sortBy=createdAt&sortDir=desc
Authorization: Bearer {{jwtToken}}
```

GET /reservations/{id}

```http
GET {{baseUrl}}/reservations/1
Authorization: Bearer {{jwtToken}}
```

POST /reservations

```http
POST {{baseUrl}}/reservations
Content-Type: application/json
Authorization: Bearer {{jwtToken}}
```

Request Body:

```json
{
    "resourceId": 1,
    "price": 150.00,
    "startTime": "2024-01-15T10:00:00Z",
    "endTime": "2024-01-15T12:00:00Z"
}
```

PUT /reservations/{id}

```http
PUT {{baseUrl}}/reservations/1
Content-Type: application/json
Authorization: Bearer {{jwtToken}}
```

Request Body:

```json
{
    "resourceId": 1,
    "price": 200.00,
    "startTime": "2024-01-15T10:00:00Z",
    "endTime": "2024-01-15T13:00:00Z"
}
```

DELETE /reservations/{id}

```http
DELETE {{baseUrl}}/reservations/1
Authorization: Bearer {{jwtToken}}
```

PATCH /reservations/{id}/status (Admin only)

```http
PATCH {{baseUrl}}/reservations/1/status?status=CONFIRMED
Authorization: Bearer {{jwtToken}}
```

Complete Postman Collection JSON

booking-system-postman-collection.json

```json
{
    "info": {
        "name": "Booking System API",
        "description": "RESTful Booking System with JWT Authentication",
        "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
    },
    "variable": [
        {
            "key": "baseUrl",
            "value": "http://localhost:8080",
            "type": "string"
        },
        {
            "key": "jwtToken",
            "value": "",
            "type": "string"
        }
    ],
    "item": [
        {
            "name": "Authentication",
            "item": [
                {
                    "name": "Login - Admin",
                    "request": {
                        "method": "POST",
                        "header": [
                            {
                                "key": "Content-Type",
                                "value": "application/json"
                            }
                        ],
                        "body": {
                            "mode": "raw",
                            "raw": "{\n    \"username\": \"admin\",\n    \"password\": \"admin123\"\n}"
                        },
                        "url": {
                            "raw": "{{baseUrl}}/auth/login",
                            "host": ["{{baseUrl}}"],
                            "path": ["auth", "login"]
                        }
                    },
                    "event": [
                        {
                            "listen": "test",
                            "script": {
                                "exec": [
                                    "pm.test(\"Status code is 200\", function () {",
                                    "    pm.response.to.have.status(200);",
                                    "});",
                                    "",
                                    "const response = pm.response.json();",
                                    "pm.environment.set(\"jwtToken\", response.token);",
                                    "console.log(\"Token set: \" + pm.environment.get(\"jwtToken\"));"
                                ]
                            }
                        }
                    ]
                },
                {
                    "name": "Login - User",
                    "request": {
                        "method": "POST",
                        "header": [
                            {
                                "key": "Content-Type",
                                "value": "application/json"
                            }
                        ],
                        "body": {
                            "mode": "raw",
                            "raw": "{\n    \"username\": \"user\",\n    \"password\": \"user123\"\n}"
                        },
                        "url": {
                            "raw": "{{baseUrl}}/auth/login",
                            "host": ["{{baseUrl}}"],
                            "path": ["auth", "login"]
                        }
                    }
                }
            ]
        },
        {
            "name": "Resources",
            "item": [
                {
                    "name": "Get All Resources (Public)",
                    "request": {
                        "method": "GET",
                        "header": [],
                        "url": {
                            "raw": "{{baseUrl}}/resources?page=0&size=10&sortBy=name&sortDir=asc",
                            "host": ["{{baseUrl}}"],
                            "path": ["resources"],
                            "query": [
                                {
                                    "key": "page",
                                    "value": "0"
                                },
                                {
                                    "key": "size",
                                    "value": "10"
                                },
                                {
                                    "key": "sortBy",
                                    "value": "name"
                                },
                                {
                                    "key": "sortDir",
                                    "value": "asc"
                                }
                            ]
                        }
                    }
                },
                {
                    "name": "Get Resource by ID",
                    "request": {
                        "method": "GET",
                        "header": [],
                        "url": {
                            "raw": "{{baseUrl}}/resources/1",
                            "host": ["{{baseUrl}}"],
                            "path": ["resources", "1"]
                        }
                    }
                },
                {
                    "name": "Create Resource (Admin)",
                    "request": {
                        "method": "POST",
                        "header": [
                            {
                                "key": "Content-Type",
                                "value": "application/json"
                            },
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "body": {
                            "mode": "raw",
                            "raw": "{\n    \"name\": \"Executive Suite\",\n    \"type\": \"ROOM\",\n    \"description\": \"Luxury executive meeting room with premium amenities\",\n    \"capacity\": 8\n}"
                        },
                        "url": {
                            "raw": "{{baseUrl}}/resources",
                            "host": ["{{baseUrl}}"],
                            "path": ["resources"]
                        }
                    }
                },
                {
                    "name": "Update Resource (Admin)",
                    "request": {
                        "method": "PUT",
                        "header": [
                            {
                                "key": "Content-Type",
                                "value": "application/json"
                            },
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "body": {
                            "mode": "raw",
                            "raw": "{\n    \"name\": \"Updated Conference Room\",\n    \"type\": \"ROOM\",\n    \"description\": \"Recently renovated with new equipment\",\n    \"capacity\": 30\n}"
                        },
                        "url": {
                            "raw": "{{baseUrl}}/resources/1",
                            "host": ["{{baseUrl}}"],
                            "path": ["resources", "1"]
                        }
                    }
                },
                {
                    "name": "Delete Resource (Admin)",
                    "request": {
                        "method": "DELETE",
                        "header": [
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "url": {
                            "raw": "{{baseUrl}}/resources/1",
                            "host": ["{{baseUrl}}"],
                            "path": ["resources", "1"]
                        }
                    }
                }
            ]
        },
        {
            "name": "Reservations",
            "item": [
                {
                    "name": "Get All Reservations",
                    "request": {
                        "method": "GET",
                        "header": [
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "url": {
                            "raw": "{{baseUrl}}/reservations?status=CONFIRMED&minPrice=50&maxPrice=500&page=0&size=10&sortBy=createdAt&sortDir=desc",
                            "host": ["{{baseUrl}}"],
                            "path": ["reservations"],
                            "query": [
                                {
                                    "key": "status",
                                    "value": "CONFIRMED"
                                },
                                {
                                    "key": "minPrice",
                                    "value": "50"
                                },
                                {
                                    "key": "maxPrice",
                                    "value": "500"
                                },
                                {
                                    "key": "page",
                                    "value": "0"
                                },
                                {
                                    "key": "size",
                                    "value": "10"
                                },
                                {
                                    "key": "sortBy",
                                    "value": "createdAt"
                                },
                                {
                                    "key": "sortDir",
                                    "value": "desc"
                                }
                            ]
                        }
                    }
                },
                {
                    "name": "Get Reservation by ID",
                    "request": {
                        "method": "GET",
                        "header": [
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "url": {
                            "raw": "{{baseUrl}}/reservations/1",
                            "host": ["{{baseUrl}}"],
                            "path": ["reservations", "1"]
                        }
                    }
                },
                {
                    "name": "Create Reservation",
                    "request": {
                        "method": "POST",
                        "header": [
                            {
                                "key": "Content-Type",
                                "value": "application/json"
                            },
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "body": {
                            "mode": "raw",
                            "raw": "{\n    \"resourceId\": 1,\n    \"price\": 150.00,\n    \"startTime\": \"2024-01-15T10:00:00Z\",\n    \"endTime\": \"2024-01-15T12:00:00Z\"\n}"
                        },
                        "url": {
                            "raw": "{{baseUrl}}/reservations",
                            "host": ["{{baseUrl}}"],
                            "path": ["reservations"]
                        }
                    }
                },
                {
                    "name": "Update Reservation",
                    "request": {
                        "method": "PUT",
                        "header": [
                            {
                                "key": "Content-Type",
                                "value": "application/json"
                            },
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "body": {
                            "mode": "raw",
                            "raw": "{\n    \"resourceId\": 1,\n    \"price\": 200.00,\n    \"startTime\": \"2024-01-15T10:00:00Z\",\n    \"endTime\": \"2024-01-15T13:00:00Z\"\n}"
                        },
                        "url": {
                            "raw": "{{baseUrl}}/reservations/1",
                            "host": ["{{baseUrl}}"],
                            "path": ["reservations", "1"]
                        }
                    }
                },
                {
                    "name": "Delete Reservation",
                    "request": {
                        "method": "DELETE",
                        "header": [
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "url": {
                            "raw": "{{baseUrl}}/reservations/1",
                            "host": ["{{baseUrl}}"],
                            "path": ["reservations", "1"]
                        }
                    }
                },
                {
                    "name": "Update Reservation Status (Admin)",
                    "request": {
                        "method": "PATCH",
                        "header": [
                            {
                                "key": "Authorization",
                                "value": "Bearer {{jwtToken}}"
                            }
                        ],
                        "url": {
                            "raw": "{{baseUrl}}/reservations/1/status?status=CONFIRMED",
                            "host": ["{{baseUrl}}"],
                            "path": ["reservations", "1", "status"],
                            "query": [
                                {
                                    "key": "status",
                                    "value": "CONFIRMED"
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```

Testing Workflow

Step 1: Setup Environment

1. Import the Postman collection
2. Create environment with baseUrl: http://localhost:8080
3. Start your Spring Boot application

Step 2: Get JWT Token

1. Execute "Login - Admin" or "Login - User"
2. The token will be automatically saved to environment variable

Step 3: Test Resources API

1. Try GET /resources (no auth required)
2. Try POST/PUT/DELETE with admin token

Step 4: Test Reservations API

1. Create a reservation
2. Get all reservations with filters
3. Update/delete reservation
4. Test status update (admin only)

Step 5: Test Role-Based Access

1. Login as user (user/user123)
2. Try to access admin endpoints - should get 403 Forbidden

Expected Responses

Success Response (200):

```json
{
    "id": 1,
    "name": "Conference Room A",
    "type": "ROOM",
    "description": "Large conference room with projector",
    "capacity": 20,
    "active": true,
    "createdAt": "2024-01-01T10:00:00Z",
    "updatedAt": "2024-01-01T10:00:00Z"
}
```

Error Response (400/403/404):

```json
{
    "timestamp": "2024-01-01T10:00:00.000+00:00",
    "message": "Access denied",
    "details": "uri=/api/reservations/1"
}
```

Pagination Response:

```json
{
    "content": [...],
    "pageable": {
        "pageNumber": 0,
        "pageSize": 10,
        "sort": {
            "sorted": true,
            "unsorted": false,
            "empty": false
        }
    },
    "totalElements": 25,
    "totalPages": 3,
    "last": false,
    "size": 10,
    "number": 0,
    "sort": {
        "sorted": true,
        "unsorted": false,
        "empty": false
    },
    "first": true,
    "numberOfElements": 10,
    "empty": false
}
```
