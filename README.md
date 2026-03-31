# Multi-Tenant Notification Service

## Overview

This project is a multi-tenant notification service built with AdonisJS and TypeScript.  
It is designed to deliver notifications across multiple channels (email, SMS, push) while ensuring strict tenant isolation and scalable asynchronous processing.

The system enables multiple organizations (tenants) to send notifications independently using shared infrastructure, with full separation of data, configuration, and delivery logic.

---

## Architecture

The application follows a layered and event-driven architecture:

Controller → Service → Queue → Worker → Provider → External Service

### Layers

**Controllers**
- Handle incoming HTTP requests
- Validate payloads
- Delegate execution to services

**Services**
- Contain business logic
- Resolve tenant context
- Determine notification channels
- Dispatch jobs to the queue

**Queue (BullMQ / Redis)**
- Handles asynchronous job processing
- Supports retries, delays, and failure recovery
- Decouples API from delivery execution

**Workers**
- Consume jobs from the queue
- Execute delivery logic per channel
- Ensure reliability and fault tolerance

**Providers**
- Abstract third-party services (Email, SMS, Push)
- Allow vendor replacement without affecting core logic

**Middleware**
- JWT authentication
- API key validation
- Tenant resolution per request

---

## Multi-Tenancy Strategy

The system uses a shared database with strict tenant scoping:

- Each resource is associated with a `tenant_id`
- All queries are filtered by tenant context
- API keys are scoped per tenant
- Prevents cross-tenant data access

This approach allows efficient scaling while maintaining strong isolation guarantees.

---

## Notification Workflow

### End-to-End Flow

Client  
↓  
API (Controller)  
↓  
Tenant Resolution (Middleware)  
↓  
Notification Service  
↓  
Persist Notification  
↓  
Dispatch Job (Queue)  
↓  
Worker  
↓  
Provider (Email / SMS / Push)  
↓  
External API  

---

## Multi-Channel Delivery

Supported channels:

- Email (SMTP or third-party providers)
- SMS (external gateways)
- Push notifications (Firebase or similar)

Each notification can target one or multiple channels simultaneously.

---

## Queue Processing

- Redis-based queue using BullMQ
- Features:
  - Retry with backoff
  - Delayed jobs
  - Failure handling
  - Scalable workers

---

## Retry and Failure Handling

- Automatic retries on transient failures
- Configurable retry policies per channel
- Failure logging for observability
- Dead-letter strategy (future improvement)

---

## Data Model

Core entities:

**Tenant**
- id
- name
- api_key
- created_at

**User (optional)**
- id
- tenant_id
- contact data (email, phone, device_token)

**Notification**
- id
- tenant_id
- recipient
- message
- channels
- status
- created_at

**NotificationLog**
- id
- notification_id
- channel
- status (sent, failed)
- provider_response
- timestamp

---

## Authentication and Security

- JWT-based authentication for dashboard/users
- API key authentication for external integrations
- Tenant-scoped access control
- Secure handling of credentials and provider secrets

---

## Tech Stack

- Backend: AdonisJS
- Language: TypeScript
- Queue: BullMQ (Redis)
- Database: PostgreSQL / SQLite
- Providers:
  - Email: SMTP / SendGrid
  - SMS: Twilio
  - Push: Firebase Cloud Messaging

---

## Request Workflow

### Notification Creation

Client (API Key)  
↓  
API Key Middleware  
↓  
Tenant Resolution  
↓  
Controller  
↓  
Notification Service  
↓  
Persist Notification  
↓  
Dispatch Queue Jobs  

---

### Worker Processing

Queue Job  
↓  
Worker  
↓  
Channel Resolution  
↓  
Provider Execution  
↓  
Update Notification Status  
↓  
Create Notification Logs  

---

## Core Features

- Multi-tenant architecture with strict isolation
- Multi-channel notification delivery
- Asynchronous processing via queue
- Retry and failure handling
- Provider abstraction layer
- Delivery tracking and logging

---

## Design Principles

- Tenant isolation by design
- Asynchronous-first architecture
- Decoupled delivery pipeline
- Idempotent job processing
- Extensible provider system

---

## Future Improvements

- Notification templates
- Per-tenant channel configuration
- User notification preferences (opt-in/out)
- Rate limiting per tenant
- Scheduling (delayed notifications)
- Webhooks for delivery status
- Monitoring and observability dashboard

---

## What This Project Demonstrates

- Multi-tenant system design
- Event-driven architecture with queues
- Integration with external providers
- Scalable and fault-tolerant backend design
- Clean separation of concerns
