# TP5 – Service-Oriented Architecture & APIs  
## Microservices Educational Management System

This project is a simple microservices-based application that manages students and academic programs.  
It includes centralized configuration, service discovery, API gateway routing, secure authentication with JWT, and inter-service communication.


## 1. Microservices Overview

### 🔹 Discovery Service (Eureka Server)
- Registers all microservices.
- Allows dynamic discovery without hard-coding ports.

### 🔹 Gateway Service (Spring Cloud Gateway)
- Single entry point for all requests.
- Routes traffic to services registered in Eureka.
- Includes Actuator for monitoring.
  
### 🔹 Config Service (Spring Cloud Config)
- Centralizes configuration for all services.
- Connected to a Git repo.
- Acts as a Eureka Client.

### 🔹 Security Service
- Handles authentication and JWT token generation.
- Validates tokens for protected endpoints.
- Basic in-memory users for testing.

### 🔹 Student Service (Java)
- CRUD operations for students.
- Uses OpenFeign to communicate with the Filière Service.
- Protected using JWT.

### 🔹 Filière Service (Python/Flask)
- CRUD operations for academic programs.
- Validates JWT tokens.
- Registered as a Eureka Client.

## 5. Testing (Postman)

- Generate a token:  
  **POST** `/login` on Security Service with `username` and `password`.
- Copy the returned `access_token`.
- For Student or Filière requests:  
  Add → **Authorization → Bearer Token → paste token**.

