# Merge Task

## Overview

The **Merge** task combines multiple columns or values into a single result. It can merge columns horizontally (combining fields), merge values vertically (joining array values), or do both. It's the opposite of the Split task.

## When to Use

Use the Merge task when you need to:

- Combine multiple columns/fields into one field
- Join array values into a delimited string
- Merge indexed columns (like `item_1`, `item_2`, `item_3`)
- Create full names from first/last name fields
- Flatten data structures
- Consolidate columns matching a pattern

## Settings

### Key / Column name

**Required** | **Type**: Text field | **Taggable**: Yes

The target location for the merged result. Use dot notation for nested paths.

### Action

**Required** | **Type**: Select dropdown | **Default**: Merge values

Choose what to merge:

- **Merge values**: Combines array values into a single string (vertical merge)
- **Merge columns**: Combines multiple columns into one (horizontal merge)
- **Merge both**: Merges columns, then joins resulting values into a string

### Key merge method

**Type**: Select dropdown | **Conditional**: Only when Action is "Merge columns" or "Merge both"

How to identify columns to merge:

- **Column key names**: Specify exact column names
- **Indexed name**: Merge numbered columns (e.g., `item_1`, `item_2`)
- **Regular expression**: Match columns with regex pattern

### Column keys that need to be merged

**Type**: Grid field | **Taggable**: Yes | **Sortable**: Yes | **Conditional**: Only when using Column key names

List exact column names to merge (in order).

### Regex to match keys

**Type**: Text field | **Taggable**: Yes | **Conditional**: Only when using Regular expression

Regex pattern to match column names (e.g., `^item_` matches columns starting with "item_").

### Indexed key

**Type**: Text field | **Taggable**: Yes | **Default**: `{*key*}_{*index*}` | **Conditional**: Only when using Indexed name

Template for indexed column names. Wildcards:
- `{*key*}` - Key/Column name
- `{*index*}` - Index number

### Index starts with

**Type**: Number | **Default**: 0 | **Conditional**: Only when using Indexed name

Starting number for index.

### Index ends with

**Type**: Number | **Conditional**: Only when using Indexed name

Ending number for index (optional). Leave empty to merge until no more matching columns found.

### Remove original key(s)?

**Type**: Switch | **Default**: No | **Conditional**: Only when Action is "Merge columns" or "Merge both"

Whether to delete original columns after merging.

### Separator

**Type**: Select dropdown | **Customizable** | **Default**: Comma | **Conditional**: Only when Action is "Merge values" or "Merge both"

Character(s) to join values with:
- Comma (`,`), Semicolon (`;`), Space, Tab (`{*tab*}`), New line (`{*nl*}`)
- **Custom**: Any string

### Merge columns type

**Type**: Column type selector | **Optional** | **Conditional**: Only when Action is "Merge columns" or "Merge both"

Apply data type transformation to merged columns before combining.

### Merge values type

**Type**: Column type selector | **Optional** | **Conditional**: Only when Action is "Merge values" or "Merge both"

Apply transformation to final merged value.

## How It Works

### Merge Values

1. Gets array from **Key / Column name**
2. Joins values with **Separator**
3. Applies value type transformation (if configured)
4. Returns string

### Merge Columns

1. Identifies columns using the configured method
2. Applies column type transformation (if configured)
3. Merges columns into target location
4. Removes originals (if configured)

### Merge Both

Combines both: merges columns into array, then joins values into string.

## Use Cases

### Create Full Name

**Configuration:**
- **Key**: `full_name`
- **Action**: Merge columns
- **Method**: Column key names (first_name, last_name)
- **Separator**: Space

**Result**: `{first_name: "John", last_name: "Doe"}` → `{full_name: "John Doe"}`

### Join Array Values

**Configuration:**
- **Key**: `tags`
- **Action**: Merge values
- **Separator**: Comma

**Result**: `{tags: ["tag1", "tag2", "tag3"]}` → `{tags: "tag1,tag2,tag3"}`

### Merge Indexed Columns

**Configuration:**
- **Key**: `items`
- **Action**: Merge columns
- **Method**: Indexed name (`item_{*index*}`)
- **Start**: 1, **End**: 3
- **Remove original**: Yes

**Result**: `{item_1: "A", item_2: "B", item_3: "C"}` → `{items: ["A", "B", "C"]}`

### Combine Address

**Configuration:**
- **Key**: `address`
- **Action**: Merge both
- **Method**: Column key names (street, city, state, zip)
- **Separator**: Comma

**Result**: Combines address fields into single comma-separated string

## Best Practices

**Choose the Right Action**: Merge values for arrays→strings, merge columns for fields→array, merge both for fields→string.

**Use Indexed Names for Patterns**: When dealing with numbered fields (item_1, item_2), use indexed name method.

**Consider Separator Choice**: Choose separators appropriate for your data (comma for CSV, space for names, etc.).

**Apply Type Transformations**: Use column/value types to clean or convert data during merge.

**Remove Originals Carefully**: Only remove original columns if you're sure you won't need them later.

## Notes

- Merge values requires an array; merge columns works with object keys
- Empty values are included in merges (can result in double separators)
- Indexed merge stops when no more matching columns are found (unless end index specified)
- Regex patterns are matched against column names (case-sensitive by default)
- Column order in merge determines output order
- Wildcards in indexed key template are case-sensitive
- Type transformations apply before merging
