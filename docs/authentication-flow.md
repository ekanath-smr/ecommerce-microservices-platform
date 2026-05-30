# Authentication & Authorization Flow

## Overview

The platform uses JWT-based authentication and role-based authorization to secure access to microservices.

Authentication is handled by the Auth Service, while authorization is enforced across downstream services using Spring Security.

---

# Components

```text
Client
  |
  v
API Gateway
  |
  v
Auth Service
  |
  v
Protected Microservices
(Product, Inventory, Cart, Order)
```

---

# User Registration Flow

```text
Client
  |
  | POST /auth/register
  v
Auth Service
  |
  | Validate Request
  | Hash Password (BCrypt)
  | Assign Roles
  | Store User
  |
  v
Generate Access Token + Refresh Token
  |
  v
Client
```

### Steps

1. User submits registration details.
2. Password is encrypted using BCrypt.
3. Roles are validated and assigned.
4. User record is stored in the database.
5. Access Token and Refresh Token are generated.
6. Tokens are returned to the client.

---

# Login Flow

```text
Client
  |
  | POST /auth/login
  v
Auth Service
  |
  | Authenticate Credentials
  |
  v
Generate JWT Tokens
  |
  v
Client
```

### Steps

1. User submits email and password.
2. Spring Security authenticates credentials.
3. Access Token and Refresh Token are generated.
4. Tokens are returned to the client.

---

# Authenticated Request Flow

```text
Client
  |
  | Authorization: Bearer <JWT>
  v
API Gateway
  |
  v
Target Microservice
  |
  v
JWT Filter
  |
  +--> Validate Signature
  +--> Validate Expiry
  +--> Check Blacklist
  +--> Extract Roles
  |
  v
Security Context
  |
  v
Controller
```

### Steps

1. Client sends JWT in Authorization header.
2. Request is routed through API Gateway.
3. JWT Filter validates the token.
4. User details and roles are extracted.
5. SecurityContext is populated.
6. Authorized request reaches controller.

---

# Refresh Token Flow

```text
Client
  |
  | POST /auth/refresh
  v
Auth Service
  |
  | Validate Refresh Token
  |
  v
Generate New Access Token
  |
  v
Client
```

### Steps

1. Client sends refresh token.
2. Auth Service validates token.
3. New access token is generated.
4. New token is returned to the client.

---

# Logout Flow

```text
Client
  |
  | POST /auth/logout
  v
Auth Service
  |
  | Add Token To Blacklist
  |
  v
Token Revoked
```

### Steps

1. Client sends access token.
2. Token is added to blacklist.
3. Future usage of the token is rejected.

---

# Role-Based Access Control

| Role  | Permissions                                 |
| ----- | ------------------------------------------- |
| USER  | Browse products, manage cart, create orders |
| ADMIN | Manage products, categories, inventory      |

### Security Rules

```text
/auth/**     -> Public

/user/**     -> USER, ADMIN

/admin/**    -> ADMIN
```

---

# Security Features

* JWT Authentication
* Refresh Token Support
* BCrypt Password Hashing
* Stateless Authentication
* Role-Based Authorization (RBAC)
* Token Blacklisting
* Method-Level Security
* Centralized Exception Handling

---

# Design Considerations

### Why JWT?

* Stateless authentication
* Scalable across multiple service instances
* No server-side session storage

### Why Refresh Tokens?

* Short-lived access tokens improve security
* Users remain authenticated without frequent logins

### Why Token Blacklisting?

* Prevents usage of revoked tokens after logout

### Future Improvements

* Redis-based distributed token blacklist
* OAuth2 / Social Login
* Refresh Token Rotation
* Session Management
* Multi-Device Login Support
