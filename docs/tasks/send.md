# Send Task

## Overview

The **Send** task pushes data to external systems through configured connections. It's the primary way to send data out from your automation workflows to APIs, databases, cloud services, or other external destinations.

## When to Use

Use the Send task when you need to:

- Send data to external APIs or web services
- Create or update records in databases
- Upload files to cloud storage
- Push data to third-party services
- Sync data to other platforms
- Trigger actions in external systems

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your workflow data to send.

- Use dot notation for nested data (e.g., `order.details`)
- Leave **empty** to send the entire root data

### Connection

**Required** | **Type**: Entity selector

Select the connection defining where and how to send data. The connection specifies:
- Target system/service (API, database, etc.)
- Authentication credentials
- Connection-specific configuration

### Retrieve response data

**Type**: Switch with nested config | **Default**: No

Whether to capture and use the response data returned by the connection.

- **No**: Send data and discard response (workflow data unchanged)
- **Yes**: Capture response as new workflow data (shows response configuration fields)

**When enabled**, inherited AbstractRequest fields appear for:
- Data extraction from response
- Error handling
- Response transformation
- Data mapping

## How It Works

1. **Prepare Data**: Retrieves data from **Key / Column name** location
2. **Validate Connection**: Ensures connection exists and supports send operation
3. **Send Request**: Executes the connection's send operation with the data
4. **Handle Response**:
   - If **Retrieve response** is off: Discards response, returns original data
   - If **Retrieve response** is on: Processes response using AbstractRequest logic, returns response data
5. **Error Handling**: Logs errors if send fails

## Use Cases

### Create API Record

**Configuration:**
- **Key**: `new_customer`
- **Connection**: Salesforce API
- **Retrieve response**: Yes (to get generated ID)

Result: Sends customer data to Salesforce, captures created record with ID.

### Update Database

**Configuration:**
- **Key**: `order_update`
- **Connection**: MySQL Database
- **Retrieve response**: No

Result: Updates database record, continues with original data.

### Upload File

**Configuration:**
- **Key**: `file_data`
- **Connection**: AWS S3
- **Retrieve response**: Yes (to get file URL)

Result: Uploads file, captures URL from response.

### Post to Webhook

**Configuration:**
- **Key**: (empty - send all data)
- **Connection**: Webhook URL
- **Retrieve response**: No

Result: Posts entire workflow data to webhook endpoint.

## Best Practices

**Select Appropriate Data**: Use Key to send only relevant data rather than entire workflow data.

**Enable Response When Needed**: Only retrieve response data when you need information from it (like generated IDs or confirmation).

**Handle Errors**: Wrap Send tasks in Attempt tasks when failures are possible and need handling.

**Test Connections**: Verify connections work correctly before using in production automations.

**Use Appropriate Connections**: Ensure the connection type matches your target system's requirements.

## Notes

- Connection must support send operation
- Empty data at specified key logs a message (not an error)
- Response data configuration only appears when Retrieve response is enabled
- Failed sends log errors but don't stop automation (unless wrapped in error handling)
- Dynamic tags supported in Key field
- Connection credentials and configuration managed separately
- Some connections may transform or validate data before sending
