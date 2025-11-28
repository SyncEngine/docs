# HTTP Webservice (Bearer Token)

## Overview

The **HTTP Bearer Token** webservice connects to HTTP endpoints using a Bearer token. It otherwise behaves like the no‑auth HTTP client.

## When to Use

Use the HTTP Bearer webservice when you need to:

- Call modern APIs with token-based authorization
- Perform service-to-service authenticated requests

## Settings

### Authentication

**Required** | **Type**: Mixed

- **Host**: Base URL.
- **Token**: Bearer token string.

### Request/Response

Same fields as the HTTP (No Auth) webservice (endpoint, transport, request method, formats).

## How It Works

1. **Client Options**: Adds `auth_bearer = token`.
2. **Requests**: Behave like HTTP (No Auth) but include the Bearer Authorization header.

## Use Cases

### OAuth2 Resource Call

**Configuration:**
- **Host**: `https://api.service.com`
- **Token**: `eyJ...`

Result: Performs authenticated requests using the provided access token.

## Best Practices

**Token Rotation**: Rotate tokens regularly.

**Scope Minimization**: Keep tokens scoped to necessary permissions.

**HTTPS**: Always enable TLS.

## Notes

- Behaves like HTTP (No Auth) with Bearer header
