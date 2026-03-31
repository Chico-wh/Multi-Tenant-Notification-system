# Multi-Tenant SaaS Backend

## Overview

This project is a multi-tenant SaaS backend built with AdonisJS and TypeScript.  
It provides a structured and scalable foundation for applications that require tenant isolation, secure authentication, and API-based integrations.

The system is designed to support multiple organizations within a single application while ensuring strict data separation and controlled access.

---

## Architecture

The application follows a layered architecture to enforce separation of concerns and maintain scalability:

Controller → Service → Model (ORM) → Database

### Layers

**Controllers**
- Handle HTTP requests and responses
- Perform input validation
- Delegate business logic to services

**Services**
- Encapsulate business rules
- Coordinate workflows (authentication, tenant creation, API key management)
- Remain independent of HTTP layer

**Models (Lucid ORM)**
- Represent database entities
- Handle persistence and querying

**Middleware**
- Responsible for cross-cutting concerns:
  - JWT authentication
  - API key validation
  - Tenant context resolution

---

## Multi-Tenancy Strategy

The system uses a shared database with tenant scoping:

- Each entity is associated with a `tenant_id`
- All queries are scoped to the current tenant
- Prevents cross-tenant data leakage

This approach balances simplicity and scalability, making it suitable for most SaaS applications.

---

## Authentication and Security

- JWT-based authentication for users
- Secure password hashing
- Middleware-based access control
- API key mechanism for external integrations

---

## API Key System

- Tenants can generate API keys
- Keys are passed via `x-api-key` header
- Each key is linked to a tenant
- Supports revocation

Future extensions:
- Scoped permissions
- Expiration policies

---

## Tech Stack

- Backend: AdonisJS
- Language: TypeScript
- ORM: Lucid
- Database: SQLite (development), PostgreSQL (production)
- Authentication: JWT

---

## Request Workflow

### User Authentication Flow

Client  
↓  
Auth Controller  
↓  
Auth Service  
↓  
User Model  
↓  
Database  
↓  
JWT Token Response  

---

### Authenticated Request Flow

Client  
↓  
JWT Middleware  
↓  
Controller  
↓  
Service  
↓  
Model  
↓  
Database  

---

### API Key Request Flow

Client (x-api-key)  
↓  
API Key Middleware  
↓  
Tenant Resolution  
↓  
Controller  
↓  
Service  
↓  
Model  
↓  
Database  

---

## Core Features

- User registration and login
- Tenant creation and isolation
- API key generation and validation
- Middleware-based request protection

---

## Design Principles

- Clear separation of concerns
- Stateless authentication
- Explicit tenant scoping
- Extensible service layer
- Minimal coupling between components

---

## Future Improvements

- Role-Based Access Control (RBAC)
- Rate limiting per tenant
- API key scopes and permissions
- Audit logging
- Background job processing (queues)

---

## What This Project Demonstrates

- Multi-tenant architecture in a real-world scenario
- Secure authentication and authorization patterns
- Scalable backend design with TypeScript
- Structured and maintainable code organization
