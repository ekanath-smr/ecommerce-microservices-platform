# E-Commerce Microservices Architecture

## High-Level Architecture

```text
                              +-------------------+
                              |      Client       |
                              +---------+---------+
                                        |
                                        v
                              +-------------------+
                              |    API Gateway    |
                              +---------+---------+
                                        |
                                        v
                              +-------------------+
                              | Service Discovery |
                              |      (Eureka)     |
                              +-------------------+

      ---------------------------------------------------------------
      |                 |                  |             |           |
      v                 v                  v             v           v

+-------------+  +-------------+  +---------------+  +---------+  +---------+
| Auth        |  | Product     |  | Inventory     |  | Cart    |  | Order   |
| Service     |  | Service     |  | Service       |  | Service |  | Service |
+-------------+  +------+------+  +-------+-------+  +----+----+  +----+----+
       |                 |                 ^               |            |
       |                 |                 |               |            |
       |                 v                 |               |            |
       |          +-------------+          |               |            |
       |          | Redis Cache |          |               |            |
       |          +-------------+          |               |            |
       |                                   |               |            |
       +-----------------------------------+---------------+------------+

Inter-Service Communication

Cart Service -------> Product Service
Cart Service -------> Inventory Service
Cart Service -------> Order Service

Order Service ------> Product Service
Order Service ------> Inventory Service

Inventory Service --> Product Service
```

---

## Request Flow

```text
Client
  |
  v
API Gateway
  |
  +--> Auth Service
  |
  +--> Product Service --> Redis Cache
  |
  +--> Inventory Service
  |
  +--> Cart Service
  |
  +--> Order Service
```

---

## Checkout Flow

```text
User Checkout
      |
      v
Cart Service
      |
      +--> Validate Product
      |
      +--> Validate Stock
      |
      +--> Create Order
      |
      +--> Reserve Inventory
      |
      +--> Confirm Sale
      |
      +--> Clear Cart
```

## Architecture Overview

The system follows a microservices-based architecture where each business capability is implemented as an independent service.

### Core Components

* **API Gateway**

    * Single entry point for client requests
    * Request routing
    * Load balancing

* **Service Discovery**

    * Dynamic service registration and lookup
    * Enables loose coupling between services

* **Auth Service**

    * JWT authentication
    * Role-based authorization
    * Refresh token support
    * Token blacklisting

* **Product Service**

    * Product catalog management
    * Hierarchical category management
    * Redis-based caching

* **Inventory Service**

    * Stock management
    * Reservation and release operations
    * Inventory transaction audit trail

* **Cart Service**

    * Shopping cart management
    * Checkout orchestration
    * Stock validation

* **Order Service**

    * Order lifecycle management
    * Idempotent order creation
    * Compensating transaction handling

* **Redis**

    * Product data caching
    * Reduces external API latency
    * Improves read performance

```
```
