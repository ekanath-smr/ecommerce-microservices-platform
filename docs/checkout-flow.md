# Checkout Flow

## Overview

The checkout process coordinates multiple microservices to create an order while maintaining inventory consistency.

The flow uses a synchronous orchestration approach with compensating transactions to handle failures.

---

# Services Involved

```text
Cart Service
     |
     +--> Product Service
     |
     +--> Inventory Service
     |
     +--> Order Service
```

### Responsibilities

| Service           | Responsibility                          |
| ----------------- | --------------------------------------- |
| Cart Service      | Checkout orchestration                  |
| Product Service   | Product validation                      |
| Inventory Service | Stock reservation and confirmation      |
| Order Service     | Order creation and lifecycle management |

---

# High-Level Checkout Flow

```text
User
  |
  v
Cart Service
  |
  +--> Validate Cart
  |
  +--> Validate Products
  |
  +--> Validate Inventory
  |
  +--> Create Order
  |
  +--> Reserve Stock
  |
  +--> Confirm Sale
  |
  +--> Clear Cart
  |
  v
Checkout Success
```

---

# Detailed Flow

## Step 1: User Initiates Checkout

```text
POST /cart/checkout
```

Cart Service:

* Loads active cart
* Verifies cart is not empty
* Verifies cart has not already been checked out

---

## Step 2: Product Validation

Cart Service calls Product Service.

```text
Cart Service
    |
    +--> Product Service
```

Validation:

* Product exists
* Product is active
* Product information is available

---

## Step 3: Inventory Validation

Cart Service calls Inventory Service.

```text
Cart Service
    |
    +--> Inventory Service
```

Validation:

* Inventory record exists
* Requested quantity is available

---

## Step 4: Create Order

Cart Service sends order request.

```text
Cart Service
    |
    +--> Order Service
```

Order Service:

* Creates order
* Stores product snapshot
* Calculates total amount
* Marks order as CREATED

---

## Step 5: Reserve Stock

Order Service requests inventory reservation.

```text
Order Service
    |
    +--> Inventory Service
```

Inventory Service:

Available Stock → Reserved Stock

Transaction Logged:

```text
STOCK_RESERVED
```

---

## Step 6: Confirm Sale

Inventory Service confirms stock consumption.

```text
Reserved Stock
      |
      v
Sold Stock
```

Transaction Logged:

```text
SALE_CONFIRMED
```

Order Status:

```text
CONFIRMED
```

---

## Step 7: Clear Cart

After successful confirmation:

```text
Cart Service
    |
    +--> Clear Cart
```

Checkout completes successfully.

---

# Idempotent Checkout

To prevent duplicate order creation:

```text
Idempotency-Key:
cart-{cartId}-v{version}
```

Benefits:

* Safe retries
* Duplicate request protection
* Network failure recovery

---

# Failure Handling

The system uses compensating transactions.

---

## Scenario: Stock Reservation Fails

```text
Create Order
      |
      v
Reserve Stock ❌
```

Result:

* Order marked FAILED
* No inventory changes applied

---

## Scenario: Sale Confirmation Fails

```text
Reserve Stock ✅
      |
      v
Confirm Sale ❌
```

Compensation:

```text
Release Reserved Stock
```

Inventory returns to original state.

---

## Scenario: Partial Success

```text
Reserve Stock ✅
Confirm Sale ✅
Order Update ❌
```

Compensation:

```text
Undo Sale
Release Stock
Mark Order Failed
```

This restores consistency across services.

---

# Inventory State Transitions

```text
Available
    |
    v
Reserved
    |
    v
Sold
```

Compensation Path:

```text
Sold
  |
  v
Reserved
  |
  v
Available
```

---

# Distributed Transaction Strategy

Current Implementation:

```text
Synchronous Orchestration
+
Compensating Transactions
```

Characteristics:

* Simple implementation
* Strong consistency
* Easier debugging
* Suitable for learning and medium-scale systems

---

# Future Evolution

Planned improvements:

```text
Kafka Event Streaming
        +
Saga Pattern
        +
Event-Driven Checkout
```

Benefits:

* Better scalability
* Improved fault tolerance
* Reduced service coupling

---

# Engineering Concepts Demonstrated

* Microservices Architecture
* Service Orchestration
* Distributed Transactions
* Compensating Transactions
* Idempotency
* Inventory Reservation Pattern
* Fault Tolerance
* OpenFeign Communication
* Resilience4j Retry & Circuit Breaker
* Domain-Driven Service Boundaries
