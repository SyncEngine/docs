# Extract Task

## Overview

The **Extract** task allows you to pull out a specific column or field from each item in a list or array. It takes a collection of items (like rows in a table) and extracts one particular field from each item, creating a new simplified list.

## When to Use

Use the Extract task when you need to:

- Pull out a single field from a list of objects (e.g., get all email addresses from a list of customers)
- Simplify complex data structures by extracting only what you need
- Create a list of values from a specific column in your data
- Transform nested data into a flat list of specific values
- Prepare data for tasks that need simple lists instead of complex objects

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your data contains the list of items you want to extract from.

- Use a simple column name (e.g., `customers`)
- Access nested data using dot notation (e.g., `response.data.customers`)
- Leave **empty** to work with the root level of your data (the entire data set)

**Examples:**
- `orders` - Extract from the "orders" array
- `api.results` - Extract from a nested "results" array inside "api"
- (empty) - Extract from the top-level data

### The column to extract

**Required** | **Type**: Text field | **Taggable**: Yes

The specific field or column you want to extract from each item in your list.

- Supports nested keys using dot notation (e.g., `user.email`)
- Leave **empty** to extract the entire item (root level)

**Examples:**
- `email` - Extract the email field from each item
- `product.name` - Extract the nested product name
- `id` - Extract just the ID values

### The target column to put results

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies where to store the extracted values in your data.

- Leave **empty** to replace the original location (uses the "Key / Column name" value)
- Use a dot-prefixed path (e.g., `.extracted_emails`) to store at the root level for the current data.
- Use dot notation to store in a nested location (e.g., `results.email_list`)

**Examples:**
- (empty) - Replace the original data at the source location
- `email_list` - Store extracted values in a new "email_list" field
- `.customer_emails` - Store at root level in "customer_emails"

## How It Works

1. The Extract task locates the data specified in **Key / Column name**
2. Verifies that the data is iterable (a list or array)
3. For each item in the list:
    - Checks if the item is iterable (an object or array)
    - Extracts the field specified in **The column to extract**
    - If an item cannot be processed, it stores `null` for that position
4. Creates a new array containing only the extracted values
5. Stores the result in the location specified by **The target column to put results** (or the original location if not specified)

**Error handling:**
- If the source data is not iterable (not a list), the task logs a message and returns data unchanged
- If an individual item cannot be processed, it uses `null` as the value for that item

## Use Cases

### Extract Email Addresses

**Scenario**: You have a list of customer objects and need just their email addresses for a mailing list.

**Configuration:**
- **Key / Column name**: `customers`
- **The column to extract**: `email`
- **The target column to put results**: `email_list`

**Input:**
```json
{
  "customers": [
    {"name": "John Doe", "email": "john@example.com", "status": "active"},
    {"name": "Jane Smith", "email": "jane@example.com", "status": "pending"}
  ]
}
```

**Output:**
```json
{
  "customers": [...],
  "email_list": ["john@example.com", "jane@example.com"]
}
```

### Extract Product IDs from Orders

**Scenario**: Extract all product IDs from a list of order items.

**Configuration:**
- **Key / Column name**: `order.items`
- **The column to extract**: `product_id`
- **The target column to put results**: `.product_ids`

**Result**: Creates a simple list of product IDs at the root level.

### Extract Nested Values

**Scenario**: Extract city names from nested address objects.

**Configuration:**
- **Key / Column name**: `contacts`
- **The column to extract**: `address.city`
- **The target column to put results**: `cities`

**Result**: Pulls out the city from each contact's nested address structure.

### Simplify API Response

**Scenario**: An API returns complex user objects, but you only need usernames.

**Configuration:**
- **Key / Column name**: `api.response.users`
- **The column to extract**: `username`
- **The target column to put results**: `usernames`

**Result**: Creates a clean list of usernames from the complex API response.

## Common Patterns

### Replace Original Data

To extract and replace the original complex data with simplified values:

- **Key / Column name**: `products`
- **The column to extract**: `name`
- **The target column to put results**: (leave empty)

This replaces the `products` array with just the product names.

### Extract to Root Level

To extract values and place them at the root of your data structure:

- **Key / Column name**: `nested.data.items`
- **The column to extract**: `value`
- **The target column to put results**: `.extracted_values`

The dot prefix (`.extracted_values`) ensures the result is stored at the root level.

### Extract Entire Items

To extract entire items (essentially copying the list):

- **Key / Column name**: `source_list`
- **The column to extract**: (leave empty)
- **The target column to put results**: `copied_list`

This duplicates the list to a new location.

## Best Practices

### Verify Your Data Structure

Before using Extract, ensure:
- The source data is actually a list or array
- Each item in the list has the field you're trying to extract
- The field names match exactly (case-sensitive)

### Use Dot Notation for Nested Data

Both the source and target support dot notation for nested structures:
- `customer.billing.address.zip` - Extract from deeply nested data
- `results.processed.emails` - Store in nested location

### Handle Missing Fields Gracefully

If some items don't have the field you're extracting:
- The task will insert `null` for those items
- The resulting array maintains the same index positions
- Consider using a filter or condition task afterward to remove nulls if needed

### Plan Your Target Location

Think about where you want the extracted data:
- Leave empty to replace the source
- Use a new name to keep both original and extracted data
- Use root-level storage (`.fieldname`) for easy access in later tasks

## Notes

- The Extract task preserves the order and index positions of items
- If an item cannot be processed, `null` is used as a placeholder
- Nested key extraction uses dot notation (e.g., `user.profile.avatar`)
- The task maintains array indexes from the original data
- You can use dynamic tags in all fields for flexible extraction
- If the source data is not a list/array, the task returns data unchanged with a log message
- Empty or missing columns in individual items result in `null` values in the output
