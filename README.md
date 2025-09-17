
BookingBackendPortal 

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
