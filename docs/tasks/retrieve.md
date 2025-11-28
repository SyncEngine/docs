# Retrieve Task

## Overview

The **Retrieve** task fetches data from external systems through configured connections. It's the primary way to pull data into your automation workflows from APIs, databases, cloud services, or other external sources.

## When to Use

Use the Retrieve task when you need to:

- Fetch data from external APIs or web services
- Query databases for records
- Download files from cloud storage
- Pull data from third-party services
- Get data from external systems for processing
- Load configuration or reference data

## Settings

### Connection

**Required** | **Type**: Entity selector

Select the connection defining where and how to retrieve data. The connection specifies:
- Source system/service (API, database, etc.)
- Authentication credentials
- Connection-specific configuration

You can select, edit, or create connections directly from this field.

### Additional inherited settings

The Retrieve task inherits configuration fields from AbstractRequest for handling the retrieved data:

- **Data extraction**: Select which part of the response to use
- **Error handling**: How to handle failed retrievals
- **Data transformation**: Apply transformations to response data
- **Data mapping**: Map response data to your desired structure
- **Filtering**: Apply conditions to filter retrieved data
- **Pagination**: Handle paginated responses

(Exact fields depend on AbstractRequest implementation)

## How It Works

1. **Validate Connection**: Ensures connection exists and supports retrieve operation
2. **Execute Retrieve**: Calls the connection's retrieve operation
3. **Process Response**: Uses AbstractRequest logic to:
   - Extract relevant data from response
   - Apply transformations
   - Map fields
   - Filter results
   - Handle pagination
4. **Return Data**: Returns processed data as workflow data
5. **Error Handling**: Logs errors if retrieve fails

## Use Cases

### Fetch API Data

**Configuration:**
- **Connection**: REST API (customers endpoint)

Result: Retrieves customer data from API into workflow.

### Query Database

**Configuration:**
- **Connection**: MySQL Database (orders query)

Result: Fetches order records from database.

### Get Cloud File

**Configuration:**
- **Connection**: AWS S3 (file storage)

Result: Downloads file data from cloud storage.

### Load Reference Data

**Configuration:**
- **Connection**: Google Sheets (product catalog)

Result: Loads product reference data for lookups.

## Best Practices

**Configure Connection Properly**: Ensure connection settings (endpoints, queries, authentication) are correct before using in automations.

**Handle Errors**: Wrap Retrieve tasks in Attempt tasks when failures are possible and need handling.

**Use Data Extraction**: Configure extraction settings to get only the data you need from responses.

**Test Connections**: Verify connections return expected data structure before building workflows around them.

**Consider Rate Limits**: Be aware of API rate limits and add appropriate delays if needed.

**Handle Pagination**: Configure pagination settings when dealing with large datasets.

## Notes

- Connection must support retrieve operation
- Failed retrievals log errors but don't stop automation (unless wrapped in error handling)
- Response data processing depends on AbstractRequest configuration
- Connection credentials and configuration managed separately
- Some connections may require specific query parameters or filters
- Retrieved data becomes the workflow data for subsequent tasks
- Dynamic tags can be used in connection configuration where supported
