# Replace Task

## Overview

The **Replace** task finds and replaces text patterns within your data. It can perform simple text replacement or use regular expressions for complex pattern matching and substitution.

## When to Use

Use the Replace task when you need to:

- Find and replace text in strings
- Clean or standardize data formats
- Remove unwanted characters or patterns
- Transform text using regex patterns
- Update URLs or paths in bulk
- Sanitize user input

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which data to perform replacements on.

- Use dot notation for nested data
- Leave **empty** to process root level data

### Replace method

**Required** | **Type**: Select dropdown | **Default**: Replace text

Choose the replacement method:

- **Replace text**: Simple text find-and-replace
- **Replace with regular expression**: Use regex patterns

### Text / Regex to find

**Required** | **Type**: Text field | **Taggable**: Yes

What to search for:

- **Replace text**: Exact text string to find
- **Replace with regex**: Regular expression pattern (without delimiters)

### Replace with

**Type**: Text field | **Taggable**: Yes

What to replace matches with. Can include:

- Static text
- Dynamic tags
- Regex capture groups (when using regex method): `$1`, `$2`, etc.

### Apply on

**Type**: Select dropdown | **Default**: Current iteration value

Scope of replacement:

- **Current iteration value**: Replace in the value itself
- **All sub values (loop)**: Replace in all nested values recursively

### In value type

**Type**: Select dropdown | **Default**: Any

Filter by value type:

- **Any**: Replace in all values
- **Strings**: Only replace in string values
- **Arrays**: Process array values
- **Objects**: Process object values

## How It Works

1. **Locate Data**: Gets data from **Key / Column name**
2. **Determine Method**:
   - **Replace text**: Uses `str_replace()` for exact matching
   - **Replace regex**: Uses `preg_replace()` for pattern matching
3. **Apply Replacement**:
   - Processes data based on **Apply on** scope
   - Filters by **In value type**
   - Replaces matches with **Replace with** value
4. **Return Data**: Returns data with replacements applied

## Use Cases

### Simple Text Replacement

**Configuration:**
- **Key**: `description`
- **Method**: Replace text
- **Find**: `oldcompany.com`
- **Replace with**: `newcompany.com`

Result: All instances of old domain replaced with new domain.

### Remove Characters

**Configuration:**
- **Key**: `phone_number`
- **Method**: Replace text
- **Find**: `-`
- **Replace with**: (empty)

Result: Removes all dashes from phone number.

### Regex Pattern Replacement

**Configuration:**
- **Key**: `text`
- **Method**: Replace with regex
- **Find**: `\b(\d{3})-(\d{3})-(\d{4})\b`
- **Replace with**: `($1) $2-$3`

Result: Formats phone numbers from "555-123-4567" to "(555) 123-4567"

### Clean Whitespace

**Configuration:**
- **Key**: `content`
- **Method**: Replace with regex
- **Find**: `\s+`
- **Replace with**: ` ` (single space)

Result: Replaces multiple spaces with single space.

### Replace in All Values

**Configuration:**
- **Key**: (empty)
- **Method**: Replace text
- **Find**: `test`
- **Replace with**: `production`
- **Apply on**: All sub values

Result: Recursively replaces "test" with "production" throughout entire data structure.

## Best Practices

**Use Simple Replace When Possible**: Text replacement is faster than regex for exact matches.

**Test Regex Patterns**: Regular expressions can have unexpected matches - test thoroughly.

**Escape Special Characters**: In regex mode, escape special regex characters if you want to match them literally.

**Consider Scope**: Choose between current value or all sub values based on your needs.

**Filter by Type**: Use value type filtering to target specific data types.

**Handle Empty Replacements**: Replacing with empty string effectively removes the matched text.

## Notes

- Replace modifies data at the specified key location
- Text method does exact string matching (case-sensitive)
- Regex method uses PHP `preg_replace()` syntax
- Regex patterns don't need delimiters (automatically added)
- Capture groups in regex can be referenced as $1, $2, etc.
- Replace with can include dynamic tags
- All sub values option processes data recursively
- Value type filtering helps target specific data types
- Empty find pattern logs an error
