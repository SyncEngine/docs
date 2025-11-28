# Index Task

## Overview

The **Index** task allows you to reorganize and reindex items in a list or array. It can either reset numeric indexes to a clean sequential order (0, 1, 2, ...) or create custom associative indexes based on field values. Think of it as a way to control how items in your list are keyed or accessed.

## When to Use

Use the Index task when you need to:

- Reset numeric indexes after filtering or removing items
- Create custom indexes based on field values (e.g., index by ID or email)
- Convert a list into a dictionary/lookup structure
- Rekey data for easier access in later steps
- Clean up sparse or non-sequential array indexes
- Transform data structure from list to associative array

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your data contains the list of items you want to reindex.

- Use a simple column name (e.g., `users`)
- Access nested data using dot notation (e.g., `response.data.items`)
- Leave **empty** to work with the root level of your data (the entire data set)

**Examples:**
- `products` - Reindex the "products" array
- `api.results` - Reindex items in a nested "results" array
- (empty) - Reindex items at the top level

### Method

**Required** | **Type**: Select dropdown

Choose how to reindex your data:

- **Numeric list**: Resets indexes to sequential numbers starting from 0 (0, 1, 2, 3, ...)
- **Associative indexes**: Creates custom indexes based on a field value using the **New index key** template

**When to use each method:**

- Use **Numeric list** when you need clean sequential indexes, especially after filtering or data manipulation
- Use **Associative indexes** when you want to access items by a specific field value (like ID or username)

### New index key

**Type**: Text field | **Taggable**: Yes | **Conditional**: Only appears when Method is "Associative indexes"

A template that defines how to create the new index for each item. You can use:

- **Field values**: Reference fields from the current row using `{{ row.fieldname }}`
- **Wildcards**: Use special placeholders for dynamic values
    - `{*key*}` - The original index/key of the item
    - `{*index*}` - The original index/key of the item (same as `{*key*}`)
- **Static text**: Combine with field values (e.g., `user_{{ row.id }}`)

**Examples:**
- `{{ row.id }}` - Use the ID field as the index
- `{{ row.email }}` - Index by email address
- `user_{{ row.id }}` - Prefix the ID (creates: "user_123", "user_456")
- `{*key*}_{{ row.status }}` - Combine original key with status
- `{{ row.country }}_{{ row.city }}` - Composite key from multiple fields

**Important**: Items where the index template results in an empty value will be appended with numeric indexes at the end.

## How It Works

### Numeric List Method

1. Locates the data specified in **Key / Column name**
2. Verifies that the data is iterable (a list or array)
3. Extracts all values and reindexes them sequentially starting from 0
4. Replaces the original data with the reindexed list

### Associative Indexes Method

1. Locates the data specified in **Key / Column name**
2. Verifies that the data is iterable (a list or array)
3. For each item in the list:
    - Evaluates the **New index key** template
    - Replaces wildcards (`{*key*}`, `{*index*}`) with the current item's original index
    - Parses tags like `{{ row.fieldname }}` to get values from the current item
    - If the resulting index is empty, marks the item to be appended with numeric index
    - If the resulting index is valid, assigns the item to that new index
4. Appends items with empty indexes at the end with numeric indexes
5. Replaces the original data with the reindexed structure

**Error handling:**
- If no method is configured, an error is logged and data returns unchanged
- For associative method, if no index key template is provided, an error is logged
- If the source data is not iterable, a log message is created and data returns unchanged

## Use Cases

### Reset Sequential Indexes

**Scenario**: After filtering data, you have gaps in your numeric indexes (0, 2, 5, 7). Reset them to a clean sequence.

**Configuration:**
- **Key / Column name**: `filtered_items`
- **Method**: Numeric list

**Input:**
```json
{
  "filtered_items": {
    "0": {"name": "Item A"},
    "2": {"name": "Item B"},
    "5": {"name": "Item C"}
  }
}
```

**Output:**
```json
{
  "filtered_items": [
    {"name": "Item A"},
    {"name": "Item B"},
    {"name": "Item C"}
  ]
}
```

### Index Users by ID

**Scenario**: Convert a list of users into a lookup dictionary keyed by user ID.

**Configuration:**
- **Key / Column name**: `users`
- **Method**: Associative indexes
- **New index key**: `{{ row.id }}`

**Input:**
```json
{
  "users": [
    {"id": "101", "name": "John", "email": "john@example.com"},
    {"id": "102", "name": "Jane", "email": "jane@example.com"}
  ]
}
```

**Output:**
```json
{
  "users": {
    "101": {"id": "101", "name": "John", "email": "john@example.com"},
    "102": {"id": "102", "name": "Jane", "email": "jane@example.com"}
  }
}
```

### Create Email Lookup

**Scenario**: Make users accessible by their email address for quick lookups.

**Configuration:**
- **Key / Column name**: `contacts`
- **Method**: Associative indexes
- **New index key**: `{{ row.email }}`

**Result**: Access any contact directly by email address (e.g., `contacts['john@example.com']`)

### Prefix with Custom Text

**Scenario**: Create indexes with a consistent prefix for clarity.

**Configuration:**
- **Key / Column name**: `products`
- **Method**: Associative indexes
- **New index key**: `product_{{ row.sku }}`

**Result**: Items indexed as "product_ABC123", "product_XYZ789", etc.

### Composite Key Indexing

**Scenario**: Create unique indexes from multiple fields.

**Configuration:**
- **Key / Column name**: `orders`
- **Method**: Associative indexes
- **New index key**: `{{ row.customer_id }}_{{ row.order_date }}`

**Result**: Items indexed as "12345_2025-11-28", "12346_2025-11-28", etc.

### Preserve Original Index

**Scenario**: Keep the original index but add context.

**Configuration:**
- **Key / Column name**: `items`
- **Method**: Associative indexes
- **New index key**: `{*key*}_{{ row.status }}`

**Result**: If original indexes were 0, 1, 2, new indexes become "0_active", "1_pending", "2_completed"

## Understanding Index Transformations

### Numeric List Transformation

Converts any array (numeric or associative) into a clean sequential numeric list:

**Before:**
```json
{
  "5": "Apple",
  "12": "Banana",
  "foo": "Cherry"
}
```

**After:**
```json
[
  "Apple",
  "Banana",
  "Cherry"
]
```

### Associative Transformation

Converts a list into an associative array using field values:

**Before:**
```json
[
  {"id": "A", "value": 10},
  {"id": "B", "value": 20}
]
```

**After (indexed by id):**
```json
{
  "A": {"id": "A", "value": 10},
  "B": {"id": "B", "value": 20}
}
```

## Best Practices

### Choose the Right Method

- **Numeric list**: When you need clean sequential access and don't care about specific keys
- **Associative indexes**: When you need to look up items by a specific identifier

### Ensure Unique Index Values

When using associative indexes:
- Make sure the field you're indexing by contains unique values
- If multiple items have the same index value, later items will overwrite earlier ones
- Consider using composite keys if single fields aren't unique enough

### Handle Missing Fields

If your index template references a field that might not exist:
- Items with missing fields will have empty indexes
- These items get appended at the end with numeric indexes
- Consider ensuring data consistency before indexing

### Use Meaningful Index Names

Create indexes that make sense for your use case:
- ✅ `{{ row.user_id }}` - Clear and purposeful
- ✅ `customer_{{ row.id }}` - Descriptive with context
- ❌ `{{ row.x }}` - Unclear and hard to debug

### Plan for Lookup Efficiency

Associative indexes are perfect for:
- Quick lookups by identifier
- Merging data from different sources
- Creating reference dictionaries
- Avoiding iteration for specific item access

## Wildcards and Tags

### Available Wildcards

- `{*key*}` - Original index/key of the item (useful for preserving context)
- `{*index*}` - Same as `{*key*}` (alternative name)

### Available Tags

- `{{ row.fieldname }}` - Access any field from the current item
- `{{ row.nested.field }}` - Access nested fields using dot notation

### Combining Wildcards and Tags

```
{*key*}_{{ row.status }}_{{ row.priority }}
```

Creates indexes like: `0_active_high`, `1_pending_medium`, etc.

## Common Patterns

### Clean Up After Filtering

1. **Filter** items based on conditions (removes items, creates gaps in indexes)
2. **Index** with "Numeric list" method (resets to 0, 1, 2...)
3. Continue with clean sequential data

### Create Lookup Dictionary

1. **Index** by unique identifier (like ID or email)
2. Use in later steps for quick access without iteration
3. Merge or enrich data from other sources

### Multi-Level Indexing

1. **Group** data by one field (e.g., category)
2. **Loop** through each group
3. **Index** items within each group by another field (e.g., ID)
4. Create nested lookup structures

## Notes

- The Index task modifies the data structure in place at the specified key location
- With **Numeric list** method, all existing keys are discarded and replaced with 0, 1, 2, ...
- With **Associative indexes**, duplicate keys will cause later items to overwrite earlier ones
- Items that produce empty index values are appended at the end with numeric indexes
- The `{{ row }}` tag provides access to the current item during index template evaluation
- Wildcards `{*key*}` and `{*index*}` reference the original item's index before transformation
- Index keys must be valid (strings or numbers, not null or empty unless handled)
- After associative indexing, your data is no longer a simple list - access requires knowing the keys
- Dynamic tags are supported in both **Key / Column name** and **New index key** fields
- The order of items is preserved during reindexing (items appear in the same sequence)