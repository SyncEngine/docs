# Split Task

## Overview

The **Split** task breaks apart string values or array data into multiple pieces. You can split a delimited string into an array, or split an array into separate named fields. It's the opposite of the Merge task.

## When to Use

Use the Split task when you need to:

- Parse delimited strings (CSV, comma-separated lists, etc.)
- Break apart full names into first/last name fields
- Split addresses into structured components
- Convert comma-separated tags into an array
- Distribute array values into individual named fields

## Settings

### Key / Column name

**Type**: Text field | **Taggable**: Yes

Specifies which part of your data contains the value to split.

- Use dot notation for nested data (e.g., `customer.address`)
- Leave **empty** for root level data

### Action

**Required** | **Type**: Select dropdown | **Default**: Split value

Choose the split operation:

- **Split value**: Split a string into an array using a separator
- **Split into column keys**: Take an array and create separate fields for each item  
- **Split value and split into column keys**: Split string, then distribute to separate fields

### Column key split method

**Type**: Select dropdown | **Conditional**: Only when Action is "Split into column keys" or "Split both"

Choose how to name the new fields:

- **Custom names**: Manually specify the name for each field
- **Indexed name**: Auto-generate field names using a pattern

### Column key names

**Type**: Grid field | **Taggable**: Yes | **Conditional**: Only when using Custom names

Define field names for each split value:

**Current value index/key**: Position in array (0-based, optional)  
**New column key name**: Name for the field

### Indexed key

**Type**: Text field | **Taggable**: Yes | **Default**: `{*key*}_{*index*}` | **Conditional**: Only when using Indexed name

Template pattern for generating field names:

- `{*key*}` - Original Key/Column name
- `{*index*}` - Item index

**Example**: `item_{*index*}` creates "item_0", "item_1", "item_2"

### Index starts with

**Type**: Number field | **Default**: 0 | **Conditional**: Only when using Indexed name

Starting number for the index in generated field names.

### Remove original key(s)?

**Type**: Switch | **Default**: No | **Conditional**: Only when Action is "Split into column keys" or "Split both"

Whether to delete the original field after splitting.

### Separator

**Type**: Select dropdown | **Customizable** | **Conditional**: Only when Action is "Split value" or "Split both"

Character(s) or pattern marking where to split:

- Comma (`,`), Semicolon (`;`), Space, Tab (`{*tab*}`), New line (`{*nl*}`)
- **Regular expression**: Custom regex pattern
- **Custom**: Any custom separator

### Split value column definition

**Type**: Column type selector | **Optional** | **Conditional**: Only when Action is "Split value" or "Split both"

Apply data type transformation to each split value (text trimming, number conversion, date parsing, etc.).

## How It Works

### Split Value

1. Retrieves string from **Key / Column name**
2. Applies split using separator (regex or standard)
3. Applies column type transformation (if configured)
4. Returns array

### Split into Column Keys

1. Retrieves array from **Key / Column name**
2. Removes original field (if configured)
3. Creates new fields using custom names or indexed pattern
4. Returns updated data

### Split Both

Combines both operations: splits string, then distributes to separate fields.

## Use Cases

### Parse Comma-Separated Tags

**Configuration:**
- **Key**: `tags`
- **Action**: Split value
- **Separator**: Comma

**Result**: `"tag1,tag2,tag3"` → `["tag1", "tag2", "tag3"]`

### Split Full Name

**Configuration:**
- **Key**: `full_name`
- **Action**: Split both
- **Separator**: Space
- **Method**: Custom names (first_name, last_name)
- **Remove original**: Yes

**Result**: `"John Doe"` → `{first_name: "John", last_name: "Doe"}`

### Parse Address

**Configuration:**
- **Key**: `address`
- **Action**: Split both
- **Separator**: Comma
- **Method**: Custom names (street, city, state, zip)
- **Column definition**: Text (trim)

**Result**: Splits "123 Main St, Springfield, IL, 62701" into structured fields

### Distribute Array

**Configuration:**
- **Key**: `items`
- **Action**: Split into column keys
- **Method**: Indexed name (`item_{*index*}`)
- **Start**: 1

**Result**: `["Apple", "Banana"]` → `{item_1: "Apple", item_2: "Banana"}`

## Best Practices

**Choose the Right Action**: Split value for parsing strings, split into keys for distributing arrays, split both for parsing and distributing in one step.

**Use Column Types**: Apply trimming or type conversion to clean up split values.

**Custom vs Indexed Names**: Use custom names for known structures (first_name, last_name), indexed names for variable-length data.

**Test Regex Patterns**: Regex separators use PHP's `preg_split()` - test with sample data.

**Handle Edge Cases**: Consider empty values, missing pieces, or inconsistent formatting.

## Notes

- Split value requires a string; split into keys requires an array
- Empty values result in an empty array
- Special separators (`{*nl*}`, `{*tab*}`) are converted to actual characters
- Regex patterns must be valid for `preg_split()`
- Index in custom names is 0-based (first item is 0)
- Indexed names create sequential fields from the starting number
- Column type transformations apply to each individual split value
- Wildcards in indexed key template are case-sensitive
