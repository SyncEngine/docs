# Group Task

## Overview

The **Group** task allows you to organize items in a list into groups based on a common field value. It takes a flat list of items and restructures them into categories, similar to how you might group rows in a spreadsheet by a column value (like grouping sales by region or products by category).

## When to Use

Use the Group task when you need to:

- Organize data by category, type, status, or any other field
- Group orders by customer, product, or status
- Categorize items by date, region, or department
- Restructure flat data into hierarchical groups
- Prepare data for aggregation or summary operations
- Create grouped reports from flat lists
- Organize API responses by a specific attribute

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your data contains the list of items you want to group.

- Use a simple column name (e.g., `orders`)
- Access nested data using dot notation (e.g., `response.data.items`)
- Leave **empty** to work with the root level of your data (the entire data set)

**Examples:**
- `transactions` - Group the "transactions" array
- `api.products` - Group items in a nested "products" array
- (empty) - Group items at the top level

### Group by

**Required** | **Type**: Text field

The field name or column that contains the value you want to group by. Each unique value in this field will become a group.

**Examples:**
- `status` - Group items by their status (e.g., "pending", "completed", "failed")
- `category` - Group products by category
- `customer_id` - Group orders by customer
- `region` - Group sales by geographic region

**Note**: Use the field name only (not the full path). The task will look for this field within each item.

### Default group key

**Type**: Text field | **Optional**

Specifies a fallback group name for items that don't have the grouping field or have an empty/null value.

- If **set**: Items without the grouping field will be placed in this default group
- If **not set**: Items without the grouping field will be **removed** from the results

**Examples:**
- `"uncategorized"` - Group items without a category here
- `"unknown"` - Place items with missing group values here
- `"other"` - Catch-all group for ungrouped items

**Use cases:**
- Handle missing or incomplete data gracefully
- Ensure no data is lost during grouping
- Create an "other" or "miscellaneous" category

## How It Works

1. The Group task locates the data specified in **Key / Column name**
2. Verifies that the data is iterable (a list or array)
3. For each item in the list:
    - Extracts the value from the **Group by** field
    - If the value is empty or missing:
        - Uses the **Default group key** if one is specified
        - Skips the item entirely if no default is specified
    - Validates that the group value is a valid key
    - Adds the item to the appropriate group
4. Creates a new structure where:
    - Each unique group value becomes a key
    - The value for each key is an array of items belonging to that group
    - Original item indexes are preserved within each group
5. Replaces the original data with the grouped structure

**Error handling:**
- If no **Group by** field is configured, an error is logged and data returns unchanged
- If the source data is not iterable, a log message is created and data returns unchanged
- If a group value is not a valid key (e.g., contains special characters), an error is logged
- Items with invalid group values are excluded from results

## Use Cases

### Group Orders by Status

**Scenario**: Organize a list of orders by their status for reporting.

**Configuration:**
- **Key / Column name**: `orders`
- **Group by**: `status`
- **Default group key**: `"unknown"`

**Input:**
```json
{
  "orders": [
    {"id": 101, "status": "pending", "total": 50},
    {"id": 102, "status": "completed", "total": 75},
    {"id": 103, "status": "pending", "total": 100},
    {"id": 104, "status": "completed", "total": 60}
  ]
}
```

**Output:**
```json
{
  "orders": {
    "pending": [
      {"id": 101, "status": "pending", "total": 50},
      {"id": 103, "status": "pending", "total": 100}
    ],
    "completed": [
      {"id": 102, "status": "completed", "total": 75},
      {"id": 104, "status": "completed", "total": 60}
    ]
  }
}
```

### Group Products by Category

**Scenario**: Organize products into categories for display on a website.

**Configuration:**
- **Key / Column name**: `products`
- **Group by**: `category`
- **Default group key**: `"uncategorized"`

**Result**: Products are organized into groups like "electronics", "clothing", "books", with any products missing a category going into "uncategorized".

### Group Transactions by Customer

**Scenario**: Organize all transactions by customer ID for customer-specific reporting.

**Configuration:**
- **Key / Column name**: `transactions`
- **Group by**: `customer_id`
- **Default group key**: (leave empty)

**Result**: Transactions are grouped by customer ID. Any transactions without a customer_id are removed.

### Group Events by Date

**Scenario**: Organize calendar events by date for a daily schedule view.

**Configuration:**
- **Key / Column name**: `events`
- **Group by**: `date`
- **Default group key**: `"unscheduled"`

**Result**: Events are grouped by date, with events missing a date placed in "unscheduled".

### Group Support Tickets by Priority

**Scenario**: Organize support tickets by priority level for triage.

**Configuration:**
- **Key / Column name**: `tickets`
- **Group by**: `priority`
- **Default group key**: `"normal"`

**Result**: Tickets grouped into "high", "normal", "low" priority buckets.

## Understanding the Output Structure

After grouping, your data structure changes significantly:

**Before Grouping** (flat list):
```json
[
  {"name": "Item 1", "type": "A"},
  {"name": "Item 2", "type": "B"},
  {"name": "Item 3", "type": "A"}
]
```

**After Grouping** (by "type"):
```json
{
  "A": [
    {"name": "Item 1", "type": "A"},
    {"name": "Item 3", "type": "A"}
  ],
  "B": [
    {"name": "Item 2", "type": "B"}
  ]
}
```

Notice:
- The list becomes an object/associative array
- Each unique group value is a key
- Values are arrays containing the grouped items
- Original indexes are preserved within groups

## Best Practices

### Choose the Right Grouping Field

Select a field that:
- Has consistent values across items
- Contains meaningful categories
- Has a reasonable number of unique values (grouping by ID might create too many groups)
- Is present in most or all items (or use a default group)

### Handle Missing Values

Decide whether to:
- **Use a default group**: Keeps all data, easier to track items without the field
- **Skip items**: Cleaner output, but you might lose data

Consider your use case and whether missing values indicate problems or are expected.

### Consider Group Value Validity

Group values must be valid object keys:
- ✅ Simple strings, numbers
- ❌ Empty strings, null, special characters

If your grouping field might contain invalid characters, consider cleaning the data first with a transformation task.

### Plan for Post-Processing

After grouping, you may want to:
- Iterate over each group separately
- Count items in each group
- Aggregate values within groups (sum, average, etc.)
- Further process each group's items

### Preserve Original Indexes

The Group task preserves original item indexes within each group. This can be useful for:
- Tracking original data order
- Referencing items by their original position
- Debugging and data validation

## Common Patterns

### Group Then Process

1. **Group** items by category
2. **Loop** through each group (using an iteration task)
3. Process each group separately

### Group and Count

1. **Group** items by status
2. Use another task to count items in each group
3. Generate summary statistics

### Nested Grouping

1. **Group** by primary category (e.g., region)
2. **Loop** through regions
3. **Group** again by secondary category (e.g., product type)
4. Create hierarchical data structure

## Notes

- The Group task transforms a flat list into a nested object structure
- Original item indexes are preserved within each group
- Group keys must be valid (no empty strings, null values, or invalid characters)
- Items with invalid or empty group values (and no default) are excluded from results
- The grouping field value is used exactly as-is (case-sensitive)
- Consider using string transformation if you need case-insensitive grouping
- After grouping, you can no longer treat the data as a simple list - it's now an object with group keys
- Use the default group strategically to avoid losing data
- Dynamic tags are supported in the **Key / Column name** field but not in **Group by** or **Default group key**
