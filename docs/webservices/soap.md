# SOAP Webservice

## Overview

The **SOAP** webservice supports WSDL mode and manual SOAP calls. It can set SOAP headers, invoke functions, and return structured array responses.

## When to Use

Use the SOAP webservice when you need to:

- Integrate with enterprise services using SOAP
- Call operations defined by WSDL contracts

## Settings

### Authentication

**Required** | **Type**: Text

- **Host**: Base URL.

### Request

**Optional** | **Type**: Mixed

- **Endpoint**: Path appended to host.
- **WSDL mode**: Toggle WSDL usage.
- **WSDL file url**: URL to WSDL (when WSDL mode is enabled).
- **Soap function from WSDL**: Function name to call.
- **Data to fill WSDL**: Parameters for the SOAP call.
- **Soap headers**: Grid of headers (`url`, `key`, `value`).

## How It Works

1. **Client**: Initializes a SOAP client with tracing enabled for easier debugging.
2. **Headers**: Builds SOAP headers and attaches them to the request.
3. **Call**: Invokes the configured SOAP function with the provided parameters.
4. **Result**: Returns the response data in a structured array.

## Use Cases

### Invoke WSDL Operation

**Configuration:**
- **WSDL mode**: Enabled
- **WSDL file url**: `https://service/wsdl`
- **Function**: `GetOrder`
- **Data**: `{ id: 123 }`

Result: Returns structured data from SOAP service.

## Best Practices

**Prefer WSDL**: Use WSDL for stable contracts and types.

**Schema Match**: Ensure `call_data` matches service expectations.

**Inspect Trace**: Use SOAP trace for troubleshooting errors.

## Notes

- SOAP headers can include optional `url`; defaults to a common namespace
