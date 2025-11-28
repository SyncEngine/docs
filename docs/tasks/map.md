# Map Task

## Overview

The **Map** task transforms data by mapping column names or values. You can rename columns, transform values using patterns, or do both. It's particularly useful for data standardization and format conversion.

## When to Use

Use the Map task when you need to:

- Rename columns/fields to match a target schema
- Transform values based on mapping rules
- Standardize data formats across different sources
- Convert codes to labels (e.g., status codes to descriptions)
- Map source data to destination field names

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which data to transform.

- Use dot notation for nested data
- Leave **empty** to map root level data

### Action

**Required** | **Type**: Select dropdown | **Default**: Map column names

Choose what to map:

- **Map column names**: Rename fields/columns
- **Map column values**: Transform values based on mapping rules
- **Map both**: Map names, then map values

### Columns to map

**Type**: Repeater field | **Conditional**: Only when Action includes mapping columns

Define column name mappings. Each mapping includes:

#### Current column key/name

**Required** | **Type**: Text field | **Taggable**: Yes

The existing field name to rename.

#### New column key/name

**Required** | **Type**: Text field | **Taggable**: Yes

The new field name.

### Values to map

**Type**: Repeater field | **Conditional**: Only when Action includes mapping values

Define value mappings. Each mapping includes:

#### Current value

**Required** | **Type**: Text field | **Taggable**: Yes

The value to find.

#### New value

**Required** | **Type**: Text field | **Taggable**: Yes

The value to replace it with.

### Default value

**Type**: Text field | **Optional** | **Taggable**: Yes | **Conditional**: Only when Action includes mapping values

Value to use when no mapping matches. Leave empty to keep original value.

### Apply on

**Type**: Select dropdown | **Default**: Current iteration value | **Conditional**: Only when Action includes mapping values

Scope of value mapping:

- **Current iteration value**: Map the value itself
- **All sub values (loop)**: Map all nested values recursively

### In value type

**Type**: Select dropdown | **Default**: Any | **Conditional**: Only when Action includes mapping values

Filter by value type to map:

- **Any**: Map all values
- **Strings**, **Arrays**, **Objects**: Target specific types

## How It Works

### Map Column Names

1. **Get Data**: Retrieves data from **Key / Column name**
2. **Apply Mappings**: For each column mapping:
   - Finds the current column name
   - Renames it to the new column name
3. **Return Data**: Returns data with renamed columns

### Map Column Values

1. **Get Data**: Retrieves data from **Key / Column name**
2. **Apply Mappings**: For each value in the data:
   - Checks if value matches any **Current value**
   - Replaces with corresponding **New value**
   - Uses **Default value** if no match (or keeps original if default not set)
3. **Return Data**: Returns data with mapped values

### Map Both

Applies column name mapping first, then value mapping.

## Use Cases

### Rename Columns

**Configuration:**
- **Key**: `customer`
- **Action**: Map column names
- **Columns to map**:
  - Current: `fname` → New: `first_name`
  - Current: `lname` → New: `last_name`

Result: `{fname: "John", lname: "Doe"}` → `{first_name: "John", last_name: "Doe"}`

### Map Status Codes

**Configuration:**
- **Key**: `order`
- **Action**: Map column values
- **Values to map**:
  - Current: `1` → New: `pending`
  - Current: `2` → New: `completed`
  - Current: `3` → New: `cancelled`

Result: `{status: "1"}` → `{status: "pending"}`

### Transform with Default

**Configuration:**
- **Key**: `data`
- **Action**: Map column values
- **Values to map**:
  - Current: `yes` → New: `true`
  - Current: `no` → New: `false`
- **Default value**: `unknown`

Result: Unmapped values become "unknown".

### Map and Rename

**Configuration:**
- **Action**: Map both
- **Columns to map**: Rename fields
- **Values to map**: Transform status codes

Result: Both column names and values are transformed.

## Best Practices

**Use Clear Mappings**: Make column and value mappings explicit and easy to understand.

**Consider Default Values**: Set defaults when not all values have explicit mappings.

**Test Thoroughly**: Verify mappings work correctly with your actual data.

**Combine Carefully**: When using "map both", ensure column mappings don't interfere with value mappings.

**Document Complex Mappings**: For many mappings, consider documenting the logic externally.

## Notes

- Map modifies data at the specified key location
- Column mappings are applied in order
- Value mappings check for exact matches
- Default value applies to unmapped values only
- If default is empty, unmapped values remain unchanged
- Apply on "all sub values" processes data recursively
- Value type filtering targets specific data types
- Dynamic tags supported in all mapping fields
