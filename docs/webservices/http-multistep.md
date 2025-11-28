# HTTP Webservice (Multistep Authorization)

## Overview

The **HTTP Multistep** webservice adds a step‑wise authorization flow on top of the HTTP client. It can perform one or more configured requests to obtain tokens or state, and parse responses from body, headers, or redirects.

## When to Use

Use the HTTP Multistep webservice when you need to:

- Implement OAuth‑like flows requiring multiple HTTP interactions
- Run custom handshake processes before calling main APIs

## Settings

### Authentication Root

Contains three main parts returned by the webservice:

1. **Variables** (params): Static values used across steps (e.g., `client_id`, `scope`).
2. **Authorization process steps** (repeater): Ordered list of step configurations (Request / Response / Actions tabs).
3. **Authorized request base config**: Default request settings applied after successful authorization (can override host and inherit standard HTTP fields like endpoint, transport, formats).

### Step: Request Tab

Each step defines:
- **Url**: Full or relative URL (taggable).
- Standard HTTP request fields (method, headers, body, formats, etc.).

### Step: Response Tab

Defines how to parse and store data:
- **Format**: Decode response before tag extraction (json, xml, etc.).
- **Tag storage grid**: Rows with:
   - **Response type**: `body` | `header` | `redirect`.
   - **Response param name**: Path/key to extract (supports `0` and nested tag parsing).
   - **Tag name**: Storage key (e.g., `access_token`).
   - **Expiration**: Timestamp, time format (`02:30:15`), natural language (`"2 hours"`), date, or dynamic tag reference (e.g., `expires_in`).

### Step: Actions Tab

Flow control on result:
- **Success** options:
   - (default) Run next step
   - Skip next step (`skip`)
   - Stop loop (`stop`)
- **Error** options:
   - (default) Run previous step
   - Restart loop from beginning (`restart`)
   - Stop loop (`stop`)

### Post‑Auth Request

After steps complete, the merged authorization config overrides host and supplies defaults for subsequent requests (same fields as standard HTTP webservice).

## How It Works

1. **Initialization**: Loads step list; retrieves current auth tag state from connection internal storage.
2. **Expiration Check**: For each step it validates whether all tags referenced by that step are present and unexpired; if still valid, the step is skipped.
3. **Execution**: Sends the HTTP request for the step, reads body, headers, and request information; decodes the body when a format is specified.
4. **Step Response Parsing**: Choose the data source (body, headers, redirect URL query) and extract values into tags; if a required value is missing, the step is treated as a failure.
5. **Expiration Parsing**: Expiration may be a timestamp, a clock time (e.g., `02:30:15`), a natural language duration (e.g., `"2 hours"`), or a value from the response; it is converted to an absolute expiration time.
6. **State Update**: Save tag values and expirations, and group tags by step reference to make future validations efficient.
7. **Flow Control**: Apply configured actions on success or error (run next, skip next, go to previous, restart from beginning, or stop).
8. **Failure Safety**: If the same step fails twice in a row, the process stops and returns detailed context for troubleshooting.
9. **Authorized Defaults**: After steps finish, the authorized configuration provides default request settings (and may override the base host) for subsequent calls.

## Use Cases

### Acquire Access Token from JSON Body

**Configuration:**
- **Request**: `POST https://auth.example.com/token` with client creds
- **Response**: `format=json`; extract `access_token` from body to tag

Result: Stores token for subsequent API calls.

### Capture Session Cookie from Headers

**Configuration:**
- **Request**: `GET https://auth.example.com/login`
- **Response type**: `header`; parse `Set‑Cookie`

Result: Persists cookie value to use with later requests.

### Extract Code from Redirect URL

**Configuration:**
- **Request**: `GET https://auth.example.com/authorize`
- **Response type**: `redirect`; read `code` from URL query

Result: Obtains authorization code for token exchange.

### Full OAuth 2.0 Multi-Step Flow (Example)

Steps:
1. **Discovery**: GET `https://auth.example.com/.well-known/openid-configuration` -> tags: `authorization_endpoint`, `token_endpoint`.
2. **Authorize**: GET `{{ tags.authorization_endpoint }}?client_id=...&response_type=code&scope=...&redirect_uri=...` (Response type: `redirect`; tag: `code`).
3. **Access Token**: POST `{{ tags.token_endpoint }}` body `{ grant_type: authorization_code, code: {{ tags.code }}, client_id: ..., client_secret: ... }` (Response type: `body`; tags: `access_token` (expiration: `expires_in`), `refresh_token`).
4. **Refresh Token** (optional auto-run when expired): POST `{{ tags.token_endpoint }}` body `{ grant_type: refresh_token, refresh_token: {{ tags.refresh_token }} }` (tags: `access_token` (expiration: `expires_in`)).

Possible flow controls:
- Step 2 error: restart from beginning to refetch endpoints.
- Step 3 success: skip the refresh step until needed.
- Step 4 success or error: stop.

Tag Storage Examples:
- `access_token` expiration row: `expiration = expires_in` (parsed duration in seconds → timestamp).
- `authorization_endpoint` no expiration (blank means treated as potentially re-used until other steps force restart).

## Best Practices

**Define Steps Clearly**: Specify request and response mapping for each step.

**Secure Storage**: Persist tokens/cookies as secrets or scoped tags.

**Format Decode**: Configure `response.format` to parse JSON/XML payloads reliably.

**Limit Steps**: Keep only essential steps; discovery can be cached if stable.

**Dynamic Expiration**: Use server-provided `expires_in` rather than hardcoding durations.

**Restart Logic**: Use `restart` sparingly—prefer explicit invalidation triggers.

**Tag Naming**: Choose clear tag names (`access_token`, `refresh_token`, `auth_code`).

**Security**: Avoid logging sensitive tokens; treat tag storage as secret context.

## Notes

- Response handling loads content first to avoid async header/info gaps
- On repeated failure of the same step, the process halts and returns context for troubleshooting
- Expiration supports timestamps, duration strings, time formats, or parsed tag values
