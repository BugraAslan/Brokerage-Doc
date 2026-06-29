# Brokerage Application

A simple brokerage platform implemented with a microservice architecture using Spring Boot.

The application allows customers to create buy/sell orders, reserve assets, update portfolios after matched orders and manage customer authorization.

## Architecture

The project consists of three independent microservices.

| Service | Responsibility |
|----------|----------------|
| customer-service | Authentication, authorization and customer info |
| order-service | (Orchestrator) Order lifecycle management |
| asset-service | Customer asset portfolio management |

```
                +--------------------+
                |   Client / REST    |
                +---------+----------+
                          |
        +-----------------+-----------------+
        |                                   |
+-------------------+             +-------------------+
|   order-service   |------------>| customer-service  |
+-------------------+             +-------------------+
        |
        |
        v
+-------------------+
|   asset-service   |
+-------------------+
```

---

# Technologies

- Java 21
- Spring Boot 4
- Spring Data JPA
- Spring Security
- Spring Validation
- Spring Cloud OpenFeign
- Resilience4j
- MySQL
- Redis
- Kafka
- Docker
- JUnit
- Mockito
- Maven

---

# Services

## customer-service

Responsible for:

- Customer authentication
- Basic Authentication
- Role management
- Authorization
- Customer information endpoint

Repository:

```
https://github.com/BugraAslan/customer-service
```

---

## order-service

Responsible for:

- Create Order 
- Match Order
- Cancel Order
- Order listing
- Order caching
- Asset listing
- Order status management
- Sent notification

Repository:

```
https://github.com/BugraAslan/order-service
```

---

## asset-service

Responsible for:

- Asset creation
- Asset reservation
- Asset release
- Asset listing
- Asset caching
- Asset synchronization after matched orders

Repository:

```
https://github.com/BugraAslan/asset-service
```

---

# Security

Authentication is implemented using HTTP Basic Authentication.

Two user roles are supported:

- ADMIN
- CUSTOMER

Authorization rules

- ADMIN can access every endpoint.
- CUSTOMER can only access resources belonging to their own customerId.

---

# Communication

Service-to-service communication is implemented using OpenFeign.

```
order-service (Saga Orchestrator)
        |
        +------> customer-service
        |
        +------> asset-service
```

Resilience is provided with:

- Retry
- Feign Error Decoder

---

# Event Driven Communication

Kafka is used for order update events.

Example flow

```
Order Matched 
      |
      v
Kafka (OrderNotificationEvent)
      |
      v
Consumers
```

---

# Idempotency

Asset operations are idempotent.

Duplicate requests are prevented using transaction records.

Examples

- Reserve Asset
- Release Asset
- Create Asset

---

# Features

- Customer authentication
- Order creation
- Order cancellation
- Order matching
- Asset reservation
- Asset release
- Portfolio management
- Retry mechanism
- Global exception handling
- Correlation ID logging
- Validation
- Unit tests

---

# Testing

The project includes

- Unit Tests
- Mockito
- JUnit 5

order-service's and asset-service's business logic unit tested.

---

# Logging

Each request is assigned a Correlation ID.

```
X-Correlation-Id
```

The Correlation ID is propagated between services for distributed tracing.

---

# Future Improvements

- API Gateway
- OAuth2 / JWT
- Distributed Tracing
- Circuit Breaker
- Zipkin
- Prometheus
- Grafana
- Kubernetes

---

# Author

Buğra Aslan
