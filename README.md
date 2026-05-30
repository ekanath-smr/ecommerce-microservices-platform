    # 🛒 E-Commerce Microservices Platform

A production-style distributed e-commerce backend built using **Java**, **Spring Boot**, and **Microservices Architecture**.

The system is designed to demonstrate real-world backend engineering concepts including **authentication and authorization**, **service discovery**, **API gateway routing**, **inter-service communication**, **distributed transaction handling**, **fault tolerance**, **caching**, and **domain-driven service decomposition**.

The platform is composed of independently deployable services responsible for authentication, product catalog management, inventory management, cart operations, and order processing.

---

# 🚀 Key Features

* Microservices-based architecture
* JWT Authentication & Authorization
* Role-Based Access Control (RBAC)
* API Gateway Pattern
* Service Discovery with Eureka
* Inter-Service Communication using OpenFeign
* Redis-based Caching
* Circuit Breaker & Retry using Resilience4j
* Idempotent Order Creation
* Distributed Transaction Compensation
* Centralized Exception Handling
* Swagger/OpenAPI Documentation
* Structured Logging
* DTO-Based API Design
* Layered Clean Architecture
* Unit Testing with JUnit & Mockito

---

# 🏗️ System Architecture

For detailed architecture diagrams and request flows:

📄 [Architecture Documentation](docs/architecture.md)

---

# 📦 Microservices

| Service           | Responsibility                                | Repository                                                 |
| ----------------- | --------------------------------------------- | ---------------------------------------------------------- |
| Auth Service      | Authentication, Authorization, JWT Management | https://github.com/ekanath-smr/ecommerce-auth-service      |
| Product Service   | Product Catalog & Category Management         | https://github.com/ekanath-smr/ecommerce-product-service   |
| Inventory Service | Stock Management & Reservation                | https://github.com/ekanath-smr/ecommerce-inventory-service |
| Cart Service      | Cart Operations & Checkout Coordination       | https://github.com/ekanath-smr/ecommerce-cart-service      |
| Order Service     | Order Lifecycle Management                    | https://github.com/ekanath-smr/ecommerce-order-service     |
| Discovery Service | Service Registration & Discovery              | https://github.com/ekanath-smr/ecommerce-discovery-service |
| API Gateway       | Request Routing & Gateway Layer               | https://github.com/ekanath-smr/ecommerce-api-gateway       |

---

# 🔄 Core Business Flow

## User Authentication

1. User registers or logs in through Auth Service.
2. JWT Access Token and Refresh Token are issued.
3. API Gateway validates and forwards authenticated requests.
4. Role-based authorization is enforced across services.

---

## Product Browsing

1. User requests product information.
2. Product Service fetches data.
3. Frequently accessed products are served from Redis Cache.
4. Product catalog supports pagination, sorting, filtering, and category hierarchy.

---

## Checkout Flow

1. User adds products to cart.
2. Cart Service validates product and inventory availability.
3. Cart Service initiates checkout.
4. Order Service creates order.
5. Inventory Service reserves stock.
6. Inventory Service confirms sale.
7. Order status is updated.
8. Cart is cleared after successful checkout.

---

## Failure Handling

If any operation fails during order creation:

* Confirmed inventory operations are rolled back.
* Reserved stock is released.
* Order is marked as FAILED.
* System maintains consistency through compensating transactions.

---

# ⚙️ Technology Stack

## Backend

* Java 21
* Spring Boot
* Spring Security
* Spring Data JPA
* Hibernate

## Databases

* MySQL
* H2 Database

## Distributed Systems

* Spring Cloud Gateway
* Eureka Service Discovery
* OpenFeign
* Resilience4j

## Caching

* Redis

## Documentation & Testing

* Swagger/OpenAPI
* JUnit 5
* Mockito

## Build Tool

* Maven

---

# 🧠 Distributed Systems Concepts Implemented

This project demonstrates practical implementation of:

* Microservices Architecture
* API Gateway Pattern
* Service Discovery Pattern
* JWT-Based Security
* Role-Based Access Control
* Distributed Transactions
* Compensating Transactions
* Idempotency
* Circuit Breaker Pattern
* Retry Pattern
* Cache-Aside Pattern
* Fault-Tolerant Service Communication
* Domain-Driven Service Boundaries

---

# 📂 Repository Structure

```text
ecommerce-microservices-platform
│
├── docs
│   └── architecture.md
│
├── Auth Service
├── Product Service
├── Inventory Service
├── Cart Service
├── Order Service
├── Discovery Service
└── API Gateway
```

---

# 📈 Engineering Highlights

* Designed a distributed e-commerce backend using independently deployable Spring Boot microservices.
* Implemented secure JWT authentication with refresh tokens and role-based authorization.
* Built Redis caching to significantly reduce product retrieval latency.
* Implemented resilient inter-service communication using OpenFeign and Resilience4j.
* Designed inventory reservation and compensation workflows to maintain consistency across services.
* Developed idempotent order creation mechanisms to prevent duplicate order processing.
* Applied clean architecture principles with DTO mapping, validation, centralized exception handling, and structured logging.

---

# 🚧 Future Enhancements

* Kafka-Based Event-Driven Architecture
* Saga Orchestration Pattern
* Docker Containerization
* Kubernetes Deployment
* Distributed Tracing (OpenTelemetry / Zipkin)
* ELK-Based Centralized Logging
* Payment Service Integration
* CI/CD Pipeline Automation

---

# 👨‍💻 Author

**Ekanath S M R**

Backend Engineer | Java | Spring Boot | Microservices

GitHub: https://github.com/ekanath-smr
