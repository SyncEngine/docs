# API Reference

The SyncEngine API allows external applications and systems to trigger automations programmatically through secure HTTP endpoints.
This integration method provides a flexible way to control SyncEngine from external workflows or data pipelines.

---

## Overview

Each automation you create in SyncEngine is assigned an **endpoint name**, which defines how it can be called via the API.
SyncEngine exposes a standardized route structure for executing automations directly.

### Route Definition

```php
#[Route(
    '/endpoint/{endpoint:endpoint}/{action:action}',
    name: 'endpoint_execute',
    defaults: ['action' => 'execute'],
    methods: ['GET', 'POST', 'TRACE']
)]
```

This means you can trigger any automation through its endpoint name, using `GET`, `POST`, or `TRACE` requests.  
SyncEngine also provides a generic endpoint at `/api/endpoint`, which lists all available endpoints and their configurations.

---

## Authentication and API Tokens

All API requests require authentication using an **API Token**.  
API tokens are created and managed through the SyncEngine web interface.

1. Open **Account → API Tokens → Create new**.
2. Optionally restrict access by **IP address**.
3. Set an **expiration date** if you want to limit token validity.

Each token can be individually revoked or replaced at any time.

Include your token in the `Authorization` header when making API requests:

```bash
Authorization: Bearer YOUR_API_TOKEN
```

Example request:

```bash
curl -X POST https://your-syncengine-domain/api/endpoint/orders/execute   -H "Authorization: Bearer YOUR_API_TOKEN"   -H "Content-Type: application/json"   -d '{"status": "new"}'
```

---

## Triggering Automations

You can start (trigger) any automation by calling its endpoint with a supported HTTP method.  
If the automation requires specific input data, you can include it in the request body.  
Otherwise, the automation may use its own **Retrieve** tasks to load necessary data automatically.

Example trigger call:

```bash
curl -X POST https://your-syncengine-domain/api/endpoint/invoice_generate/execute   -H "Authorization: Bearer YOUR_API_TOKEN"   -H "Content-Type: application/json"   -d '{"invoice_id": 12345}'
```

This request starts the automation with endpoint name `invoice_generate` and passes the provided data to the first task.

---

## Listing Available Endpoints

To see which automations can be triggered externally, call:

```
GET /api/endpoint
```

This returns a JSON list of all public endpoints.

Example response:

```json
[
  {
    "endpoint": "invoice_generate", 
    "name": "Generate invoice",
    "description": "Generates and sends invoices automatically",
    "link": "https://your-syncengine-domain/api/endpoint/invoice_generate"
  },
  {
    "endpoint": "sync_orders",
    "name": "Synchronize orders",
    "description": "Synchronizes orders from the e-commerce platform",
    "link": "https://your-syncengine-domain/api/endpoint/sync_orders"
  }
]
```

---

## Security Configuration

SyncEngine’s API security is managed using Symfony’s built-in security system, defined in `security.yaml`.  
This configuration controls password hashing, token authentication, role permissions, and IP-based access.

This configuration ensures that:
- API requests are stateless and verified through `ApiTokenAuthenticator`.
- Tokens are validated per request and tied to user roles.
- Access can be restricted by IP or by user role (`ROLE_USER`, `ROLE_ADMIN`).

---

## Responses

When triggering automations, the API provides structured JSON feedback.

## Summary

- Each automation defines an **endpoint name** for triggering via `/api/endpoint/{endpoint}/execute`.
- `/api/endpoint` lists all available automations with their endpoints.
- API Tokens are required for authentication and can be restricted by IP or expiration.
- SyncEngine uses Symfony’s security framework for role-based and IP-based access control.
- Request data can be passed directly or retrieved within the automation.

By using the SyncEngine API, developers can securely integrate and automate external systems with minimal configuration.
