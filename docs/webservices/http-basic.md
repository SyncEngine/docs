# HTTP Webservice (Basic Auth)

## Overview

The **HTTP Basic** webservice connects to HTTP endpoints using Basic Authentication. It otherwise behaves like the no‑auth HTTP client.

## When to Use

Use the HTTP Basic webservice when you need to:

- Integrate services that expect Basic auth (key/secret)
- Access legacy endpoints protected with username/password

## Settings

### Authentication

**Required** | **Type**: Mixed

- **Host**: Base URL.
- **Username / Key**: Identifier.
- **Password / Secret**: Optional (blank password is supported).

### Request/Response

Same fields as the HTTP (No Auth) webservice (endpoint, transport, request method, formats).

## How It Works

1. **Client Options**: Adds `auth_basic = [username, password]`.
2. **Requests**: Behave like HTTP (No Auth) but include the Basic Authorization header.

## Use Cases

### Vendor API with Basic Auth

**Configuration:**
- **Host**: `https://api.vendor.com`
- **Username**: `api_key`
- **Password**: `secret`

Result: Performs authenticated calls to vendor endpoints.

## Best Practices

**Secrets**: Store credentials securely via connection secrets.

**HTTPS**: Always use TLS to protect credentials.

**Empty Password**: Supported; use when the provider requires only a key.

## Notes

- Behaves like HTTP (No Auth) with Basic headers
