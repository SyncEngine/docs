# HTTP Webservice (No Auth)

## Overview

The **HTTP** webservice connects to HTTP endpoints without authentication. It supports common methods, request transports (body, headers, query), and format encode/decode.

## When to Use

Use the HTTP webservice when you need to:

- Call public APIs without authentication
- Send webhooks with JSON or form data
- Perform health checks or simple GET requests

## Settings

### Authentication

**Required** | **Type**: Text

- **Host**: Base URL for the service (e.g., `https://api.example.com`).

### Common

**Type**: Text

- **Endpoint**: Path appended to host (e.g., `/v1/items`).

### Retrieve

**Optional** | **Type**: Mixed

- **Send current data**: If enabled, include current workflow data in the request.
- **Transport**: Where to place included data: `body`, `headers`, `query`, or `custom`.
- **Request method**: HTTP method (default `GET`).
- **Request format**: Encode body (JSON, form, xml, etc.).
- **Response format**: Decode response content (JSON, xml, etc.).

### Send

**Optional** | **Type**: Mixed

- **Transport**: Where to place outgoing data (default `body`).
- **Request method**: HTTP method (default `POST`).
- **Request/Response format**: As in Retrieve.

## How It Works

1. **Build URL**: Concatenates `host + endpoint`.
2. **Prepare Options**: Client options derived from request config (timeouts omitted if empty).
3. **Retrieve**: Executes request; optionally merges current data into selected transport; decodes response if configured; returns content and debug info.
4. **Send**: Executes request (default `POST`); places provided data into selected transport; decodes response if configured.
5. **Errors**: Failures include request context to aid troubleshooting.

## Use Cases

### List Resources

**Configuration:**
- **Host**: `https://api.example.com`
- **Endpoint**: `/items`
- **Request method**: `GET`
- **Transport**: `query`

Result: Retrieves items with query filters.

### Post Webhook

**Configuration:**
- **Host**: `https://hooks.example.com`
- **Endpoint**: `/incoming`
- **Request method**: `POST`
- **Transport**: `body`
- **Request format**: `json`

Result: Sends event payload to webhook endpoint.

## Best Practices

**Base URL and Paths**: Keep `host` without trailing slash; use leading `/` for `endpoint`.

**Transport Choice**: Prefer `body` for structured payloads; use `query` for simple filters.

**Formats**: Set `request.format` when sending structured bodies to ensure correct content type; configure `response.format` to decode content.

**Timeouts**: Only set when necessary; empty removes the option.

## Notes

- Response processing depends on configured formats
- Errors include request debug info for troubleshooting
