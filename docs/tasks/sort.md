# Sort Task

## Overview

The **Sort** task reorders items in a list or array based on specified criteria. You can sort by field values, use multiple sort criteria, and control the sort direction (ascending or descending).

## When to Use

Use the Sort task when you need to:

- Order lists alphabetically or numerically
- Sort records by date, priority, or other fields
- Arrange data for display or reporting
- Prioritize items based on criteria
- Organize data before processing

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your data contains the list to sort.

- Use dot notation for nested data (e.g., `order.items`)
- Leave **empty** to sort root level data

### Sortable columns

**Required** | **Type**: Repeater field

Define sort criteria. Items are sorted by the first criterion, then by subsequent criteria for ties. Each sort column includes:

#### Column key / name

**Required** | **Type**: Text field | **Taggable**: Yes

The field name to sort by. Use dot notation for nested fields.

#### Order

**Type**: Select dropdown | **Default**: Ascending

Sort direction:

- **Ascending**: A-Z, 0-9, oldest to newest
- **Descending**: Z-A, 9-0, newest to oldest

### Keep index association?

**Type**: Switch | **Default**: No

Whether to preserve original array keys after sorting.

- **No**: Reindexes array (0, 1, 2...)
- **Yes**: Maintains original keys (useful for associative arrays)

## How It Works

1. **Locate Data**: Retrieves array from **Key / Column name**
2. **Validate**: Ensures data is iterable (array/list)
3. **Build Sort Criteria**: Constructs multi-level sort based on configured columns
4. **Sort Items**: Orders items using comparison logic:
   - Compares first sort column
   - If equal, uses next sort column
   - Continues through all criteria
5. **Reindex**: If Keep index association is off, reindexes array
6. **Return Data**: Returns sorted array

## Use Cases

### Sort by Single Field

**Configuration:**
- **Key**: `customers`
- **Sortable columns**:
  - Column: `last_name`, Order: Ascending

Result: Customers sorted alphabetically by last name.

### Multi-Level Sort

**Configuration:**
- **Key**: `orders`
- **Sortable columns**:
  - Column: `priority`, Order: Descending
  - Column: `date`, Order: Ascending

Result: Orders sorted by priority (high to low), then by date (old to new) for same-priority items.

### Sort by Nested Field

**Configuration:**
- **Key**: `products`
- **Sortable columns**:
  - Column: `pricing.amount`, Order: Ascending

Result: Products sorted by nested pricing amount.

### Sort by Date

**Configuration:**
- **Key**: `events`
- **Sortable columns**:
  - Column: `event_date`, Order: Descending

Result: Events sorted by date, newest first.

## Best Practices

**Use Multiple Criteria**: Add secondary sort columns to handle ties consistently.

**Choose Correct Direction**: Ascending for A-Z/low-to-high, descending for Z-A/high-to-low.

**Sort Before Processing**: Sort data early in workflows when order matters for subsequent tasks.

**Consider Data Types**: Ensure fields contain comparable data types (all numbers or all strings).

**Test with Sample Data**: Verify sort behavior matches expectations, especially with mixed data types.

## Notes

- Sortable columns are applied in order (first has priority)
- Empty or missing fields are typically sorted to one end
- String comparison is case-sensitive by default
- Numeric strings are compared as strings unless converted
- Keep index association preserves associative array keys
- Original array is replaced with sorted version
- Sorting doesn't modify item contents, only order
- Dynamic tags supported in Key and Column fields
