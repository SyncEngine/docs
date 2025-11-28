# Set Task

## Overview

The **Set** task creates, updates, or modifies data fields in your workflow. It's one of the most fundamental tasks for data manipulation, enabling you to add fields, update values, remove fields, or apply schema transformations.

## When to Use

Use the Set task when you need to:

- Add new fields to your data
- Update existing field values
- Set default or fallback values
- Remove specific fields
- Apply schema definitions to structure data
- Calculate and set derived values
- Clean or standardize data formats

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your data to modify.

- Use dot notation for nested data (e.g., `order.details`)
- Leave **empty** to modify root level data

### Reorder data?

**Type**: Switch | **Default**: No

Whether to move set fields to the end of the data structure.

### Force if invalid?

**Type**: Switch | **Default**: No

Whether to create a new data collection if target data is scalar (single value) instead of object/array.

### Set method

**Required** | **Type**: Select dropdown | **Default**: Set column values

Choose what to set:

- **Set column values**: Manually define field names and values
- **Set schema**: Apply a schema definition to structure data
- **Set both**: Apply both schema and manual values

### Schema

**Type**: Entity selector | **Conditional**: Only when Set method is "Set schema" or "Set both"

Select a schema to apply. Schemas define field names, types, and validation rules.

### Columns

**Type**: Repeater field | **Conditional**: Only when Set method is "Set column values" or "Set both"

Define fields to set. Each column includes:

#### Column key / name

**Required** | **Type**: Text field | **Taggable**: Yes

The field name to create or update. Supports dot notation for nested paths.

#### Source

**Type**: Select dropdown | **Default**: Set from data

Where the value comes from:

- **Set from data**: Use value from current workflow data
- **Manual value**: Enter a specific value
- **Unset/Remove**: Delete this field

#### Source column key / name

**Type**: Text field | **Taggable**: Yes | **Conditional**: Only when Source is "Set from data"

Which data field to copy the value from. Leave empty to use the same name as Column key.

#### Value

**Type**: Text field | **Taggable**: Yes | **Conditional**: Only when Source is "Manual value"

The value to set. Can include dynamic tags.

#### Column type

**Type**: Column type selector | **Optional**

Apply data type transformation (text formatting, number conversion, date parsing, etc.).

## How It Works

1. **Determine Target**: Gets target location from **Key / Column name**
2. **Validate Target**: Checks if target is iterable or uses Force setting
3. **Remove Old Fields**: If Reorder is on, removes existing fields
4. **Apply Schema**: If schema selected, applies schema definitions
5. **Set Columns**: For each column:
   - Determines source (data field, manual value, or unset)
   - Applies column type transformation
   - Sets or removes the field
6. **Return Data**: Returns updated workflow data

## Use Cases

### Add Calculated Field

**Configuration:**
- **Method**: Set column values
- **Columns**:
  - Column key: `total_with_tax`
  - Source: Manual value
  - Value: `{{ data.subtotal * 1.2 }}`

### Set Default Values

**Configuration:**
- **Method**: Set column values
- **Columns**:
  - Column key: `status`, Source: Manual, Value: `"pending"`
  - Column key: `created_at`, Source: Manual, Value: `{{ "now"|date }}`

### Copy and Transform

**Configuration:**
- **Method**: Set column values
- **Columns**:
  - Column key: `email_lower`
  - Source: Set from data
  - Source column: `email`
  - Column type: Text (lowercase)

### Apply Schema

**Configuration:**
- **Method**: Set schema
- **Schema**: customer_schema

Result: Applies schema definitions to structure and validate data.

### Remove Fields

**Configuration:**
- **Method**: Set column values
- **Columns**:
  - Column key: `temp_data`, Source: Unset/Remove

## Best Practices

**Use Schemas for Consistency**: When working with structured data that appears frequently, create schemas for consistent validation and typing.

**Leverage Column Types**: Use column type transformations to clean and standardize data during the set operation.

**Be Careful with Reorder**: Only enable reorder when field position matters for your output format.

**Use Manual Values for Constants**: Set static values, defaults, or calculated values using manual source.

**Copy and Transform**: Use "Set from data" with column types to copy and transform fields in one step.

## Notes

- Set modifies data at the specified key location
- Empty column key with schema applies schema to current level
- Reorder removes and re-adds fields (changing their position)
- Force creates new collection if target is scalar
- Column types apply transformations during set
- Dynamic tags supported in all value fields
- Unset/Remove deletes fields completely
- Schema validation errors are logged
- Multiple columns are processed in order
