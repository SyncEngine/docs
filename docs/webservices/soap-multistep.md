# SOAP Webservice (Multistep Authorization)

## Overview

The **SOAP Multistep** webservice adds a step‑wise authorization flow on top of the SOAP client. It can perform one or more configured SOAP requests to obtain tokens or session state, and parse responses into reusable tags for subsequent steps.

## When to Use

Use the SOAP Multistep webservice when you need to:

- Implement multi-step SOAP authentication flows (session tokens, context initialization)
- Execute preliminary SOAP calls to establish credentials before main operations

## Settings

### Authentication Root

Contains three main parts:

1. **Variables** (params): Static values used across steps (e.g., `client_id`, `service_key`).
2. **Authorization process steps** (repeater): Ordered list of step configurations (Request / Response / Actions tabs).
3. **Authorized request base config**: Default request settings applied after successful authorization (can override host and inherit standard SOAP fields).

### Step: Request Tab

Each step defines:
- **Url**: Full or relative URL (taggable).
- Standard SOAP request fields:
  - **Endpoint**: Path appended to host.
  - **WSDL mode**: Toggle WSDL usage.
  - **WSDL file url**: URL to WSDL (when enabled).
  - **Soap function from WSDL**: Function name to invoke.
  - **Data to fill WSDL**: Parameters for the SOAP call.
  - **Soap headers**: Grid of headers (`url`, `key`, `value`).

### Step: Response Tab

Defines how to parse and store data:
- **Format**: Decode response before tag extraction (if applicable).
- **Tag storage grid**: Rows with:
  - **Response param name**: Path/key to extract from SOAP response (supports nested tag parsing).
  - **Tag name**: Storage key (e.g., `session_token`).
  - **Expiration**: Timestamp, time format (`02:30:15`), natural language (`"2 hours"`), or dynamic tag reference (e.g., `expires_in`).

### Step: Actions Tab

Flow control on result:
- **Success** options:
  - (default) Run next step
  - Skip next step
  - Stop loop
- **Error** options:
  - (default) Run previous step
  - Restart loop from beginning
  - Stop loop

### Post‑Auth Request

After steps complete, the merged authorization config overrides host and supplies defaults for subsequent SOAP requests.

## How It Works

1. **Initialization**: Loads the configured steps and the current authorization state (stored tags and expirations).
2. **Expiration Check**: For each step, verifies whether the step's required tags exist and are still valid; skips steps that are already satisfied.
3. **Step Request**: Creates a SOAP client with tracing enabled, sets SOAP headers, and invokes the configured function with provided parameters.
4. **Response Parsing**: Returns the raw SOAP response and extracts values into tags based on the configured param paths; if a required value is missing, the step is treated as a failure.
5. **Expiration Handling**: Supports timestamps, clock times, natural language durations, and dynamic response fields. Values are converted to an absolute expiration time.
6. **State Update**: Saves tag values and expirations, and groups tags by step reference to make future validations efficient.
7. **Flow Control**: Applies configured actions on success or error (run next, skip next, go to previous, restart from beginning, or stop).
8. **Failure Safety**: If the same step fails twice in a row, the process stops and returns detailed context for troubleshooting.
9. **Authorized Defaults**: After steps finish, the authorized configuration provides default request settings (and may override the base host) for subsequent SOAP calls.

## Use Cases

### Session Initialization

**Configuration:**
- **Step 1**: Call `InitSession` function to receive `session_id` and `session_token`.
- **Tag storage**: Extract `session_id` (no expiration), `session_token` (expiration: `"1 hour"`).

Result: Stores session credentials for later authenticated operations.

### Multi-Step Authentication Flow

Steps:
1. **Discovery**: SOAP call to `GetServiceEndpoint` -> tags: `auth_endpoint`.
2. **Authenticate**: Call `{{ tags.auth_endpoint }}/Login` with credentials -> tags: `access_token` (expiration: `expires_in`).
3. **Refresh Token** (auto-run when expired): Call `RefreshToken` with `access_token` -> update `access_token`.

Possible flow controls:
- Step 2 error: restart from beginning to refetch endpoints.
- Step 3 success: stop (no further steps).

## Best Practices

**Define Steps Clearly**: Specify request and response mapping for each SOAP step.

**Contracts**: Ensure WSDL and call data match the expected schema.

**State Management**: Persist step outputs securely for later steps.

**Dynamic Expiration**: Use server-provided expiration values rather than hardcoding durations.

**Tag Naming**: Choose clear tag names (`session_token`, `auth_context`, `service_key`).

## Notes

- Experimental; interface may change. Validate thoroughly.
- Response handling returns raw SOAP response for flexible tag extraction.
- On repeated failure of the same step, the process halts and returns context for troubleshooting.
- Expiration supports timestamps, duration strings, time formats, or parsed tag values.
