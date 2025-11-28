# SQL Webservice

## Overview

The **SQL** webservice executes queries against databases using common drivers. It supports different fetch modes and key remapping.

## When to Use

Use the SQL webservice when you need to:

- Read data from tables or views
- Execute write operations (INSERT/UPDATE/DELETE)

## Settings

### Authentication

**Required** | **Type**: Mixed

- **Host**: Database host.
- **Driver**: Choose the database driver used by your environment.
- **Database**: Target database name.
- **Username / Password**: Credentials.
- **Port**: Default `3306` (help text includes common DB ports).

### Request

**Required** | **Type**: Code

- **Query**: SQL to execute.

### Retrieve Options

**Optional** | **Type**: Mixed

- **Fetch method**: `''` associated rows or `pair` key=>value.
- **Key column**: Column used as associative key (assoc mode only).

## How It Works

1. **Connect**: Creates a database connection using the selected driver.
2. **Retrieve**: Executes query and returns rows; supports `pair` fetch and `key_column` remapping.
3. **Send**: Executes query; returns success or affected results.
4. **Errors**: Connection or query failures include clear error messages to help troubleshooting.

## Use Cases

### Fetch Rows for Processing

**Configuration:**
- **Driver**: `pdo`
- **Query**: `SELECT * FROM orders WHERE status='new'`

Result: Returns rows for downstream tasks.

### Key/Value Lookup

**Configuration:**
- **Driver**: `mysqli`
- **Fetch**: `pair`
- **Query**: `SELECT sku, price FROM catalog`

Result: Produces map of `sku => price`.

## Best Practices

**Parameterized Queries**: Prefer prepared statements in `PDO` for dynamic input.

**Charset**: Ensure UTF‑8 and proper DB collation.

**Key Mapping**: Verify `key_column` exists in result set.

## Notes

- Driver choice affects behavior and error surfaces
