# Filter Task

## Overview

The **Filter** task allows you to filter items in a list or array based on conditions you define. It evaluates each row against your conditions and keeps only the rows that match (or don't match, depending on your settings). Think of it as a way to remove unwanted items from your data, similar to filtering rows in a spreadsheet.

## When to Use

Use the Filter task when you need to:

- Remove items from a list that don't meet specific criteria
- Keep only rows that match certain conditions
- Filter out invalid, incomplete, or unwanted data
- Process only specific items based on their values
- Clean up data before sending it to another system
- Select a subset of items for further processing

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your data contains the list you want to filter.

- Use a simple column name (e.g., `orders`)
- Access nested data using dot notation (e.g., `response.items`)
- Leave **empty** to filter the root level of your data (iterate over the entire data set)

**Examples:**
- `customers` - Filter the "customers" array
- `api.results` - Filter items in a nested "results" array
- (empty) - Filter items at the top level

### Filter method

**Required** | **Type**: Select dropdown | **Default**: Keep valid rows

Choose how to apply the filter based on your conditions:

- **Keep valid rows**: Keeps items that **match** the conditions (removes non-matching items)
- **Keep invalid rows**: Keeps items that **don't match** the conditions (removes matching items)

Use "Keep invalid rows" when you want to identify and keep the exceptions rather than the normal cases.

### Conditions

**Required** | **Type**: Conditions field | **Taggable**: Yes

Define the conditions that determine which rows to keep or remove. Each row in your data is evaluated against these conditions.

**Source options:**

The conditions validator provides several data sources to check against:

- **Row** (default, `{{ row }}`): Check values within the current row being evaluated
- **Key/Index** (`{{ index }}`): Check the position or key of the current row
- **Data** (`{{ data }}`): Access the full data set from previous steps
- **Cache** (`{{ cache }}`): Compare against cached values
- **Variables** (`{{ variables }}`): Use workflow variables in conditions

The source is **customizable**, allowing you to reference any data point.

**How it works:**
- Each item in your list is evaluated one at a time
- The current item is available as `{{ row }}` in your conditions
- The current item's position/key is available as `{{ index }}`
- You can use dynamic tags in condition values

**Example conditions:**
- `{{ row.status }}` equals `"active"` - Keep only active items
- `{{ row.amount }}` is greater than `100` - Keep items over $100
- `{{ row.email }}` is not empty - Keep rows with email addresses
- `{{ index }}` is less than `10` - Keep only the first 10 items

## How It Works

1. The Filter task locates the data specified in **Key / Column name**
2. Verifies that the data is iterable (a list or array)
3. Determines whether to keep valid or invalid rows based on **Filter method**
4. For each item in the list:
    - Makes the current row available as `{{ row }}` and its index as `{{ index }}`
    - Evaluates the **Conditions** against the current row
    - Marks the row as valid or invalid based on the conditions result
    - Removes rows that don't match the filter method setting
5. Creates a filtered array with only the kept items
6. For numeric arrays (lists), reindexes the array to remove gaps
7. For associative arrays (objects), preserves the original keys
8. Replaces the original data with the filtered results

**Error handling:**
- If no conditions are configured, an error is logged
- If the data key is not found, an error is logged and data is returned unchanged
- If the data is not iterable, a log message is created and data is returned unchanged

## Use Cases

### Filter Active Customers

**Scenario**: Keep only customers with "active" status for a marketing campaign.

**Configuration:**
- **Key / Column name**: `customers`
- **Filter method**: Keep valid rows
- **Conditions**: `{{ row.status }}` equals `"active"`

**Input:**
```json
{
  "customers": [
    {"name": "John", "status": "active"},
    {"name": "Jane", "status": "inactive"},
    {"name": "Bob", "status": "active"}
  ]
}
```

**Output:**
```json
{
  "customers": [
    {"name": "John", "status": "active"},
    {"name": "Bob", "status": "active"}
  ]
}
```

### Remove Low-Value Orders

**Scenario**: Filter out orders below $50 to focus on significant purchases.

**Configuration:**
- **Key / Column name**: `orders`
- **Filter method**: Keep valid rows
- **Conditions**: `{{ row.total }}` is greater than or equal to `50`

**Result**: Only orders with totals of $50 or more remain in the data.

### Identify Failed Transactions

**Scenario**: Find and keep only transactions that failed for review.

**Configuration:**
- **Key / Column name**: `transactions`
- **Filter method**: Keep invalid rows (inverse filtering)
- **Conditions**: `{{ row.status }}` equals `"success"`

**Result**: Only failed transactions (those that don't match "success") are kept.

### Filter by Multiple Conditions

**Scenario**: Keep only premium customers from the US who made purchases this month.

**Configuration:**
- **Key / Column name**: `customers`
- **Filter method**: Keep valid rows
- **Conditions**: 
    - `{{ row.tier }}` equals `"premium"` AND
    - `{{ row.country }}` equals `"US"` AND
    - `{{ row.last_purchase }}` is after `"2025-11-01"`

**Result**: Only customers matching all three criteria are kept.

### Filter by Position

**Scenario**: Keep only the first 5 items in a list.

**Configuration:**
- **Key / Column name**: `products`
- **Filter method**: Keep valid rows
- **Conditions**: `{{ index }}` is less than `5`

**Result**: Only the first 5 products remain (indexes 0-4).

## Filter Method Comparison

### Keep Valid Rows (Standard Filtering)

Use this when you want to **keep items that match** your conditions.

**Example**: Keep products that are in stock
- **Condition**: `{{ row.in_stock }}` equals `true`
- **Result**: Only in-stock products remain

### Keep Invalid Rows (Inverse Filtering)

Use this when you want to **remove items that match** your conditions (keep everything else).

**Example**: Remove completed tasks
- **Condition**: `{{ row.status }}` equals `"completed"`
- **Result**: All non-completed tasks remain

This is useful for finding exceptions or problems in your data.

## Available Data in Conditions

Within the filter conditions, you have access to:

- `{{ row }}` - The current item being evaluated (default source)
- `{{ row.fieldname }}` - Access specific fields in the current row
- `{{ index }}` - The index/key of the current row
- `{{ data }}` - The full data set from previous steps
- `{{ cache }}` - Cached values from earlier in the workflow
- `{{ variables }}` - Workflow variables

## Best Practices

### Use Descriptive Conditions

Make your conditions clear and specific:
- ✅ `{{ row.order_total }}` is greater than `1000`
- ❌ `{{ row.x }}` equals `1`

### Consider Using Inverse Filtering

Sometimes it's clearer to define what you **don't** want:
- To keep active users: Use "Keep valid rows" with condition `status = active`
- To remove test data: Use "Keep invalid rows" with condition `email contains test`

### Test with Sample Data

Before running on live data:
- Test your conditions with a small sample
- Verify both matching and non-matching items behave as expected
- Check edge cases (empty values, null, missing fields)

### Combine Multiple Conditions

Use AND/OR logic to create sophisticated filters:
- AND: All conditions must be true
- OR: At least one condition must be true
- Mix and nest conditions for complex filtering logic

### Check Array Type Behavior

- **Lists** (numeric arrays): Reindexed after filtering (0, 1, 2, ...)
- **Objects** (associative arrays): Keys are preserved

## Notes

- The Filter task modifies the data in place at the specified key location
- Items are removed permanently from the filtered array (not hidden)
- For numeric arrays, indexes are resequenced to remove gaps (0, 1, 2, etc.)
- For associative arrays (objects), original keys are preserved
- The `{{ row }}` tag represents the current item during condition evaluation
- The `{{ index }}` tag provides the item's column key or numeric position
- You can use complex nested conditions (AND, OR groups)
- Dynamic tags in conditions are evaluated for each row
- Empty or missing fields in conditions may cause unexpected results - test thoroughly
- If the source data is not a list/array, no filtering occurs and data is returned unchanged
