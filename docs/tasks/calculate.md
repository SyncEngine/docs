# Calculate Task

## Overview

The **Calculate** task performs math on your data. Each row in the task calculates one value with an expression (for example `data.total / data.quantity`) and stores the result in a field of your choice.

Tags can't calculate (`{{ data.total }} / 2` results in the text `100 / 2`), so use this task whenever you need a computed number.

## When to Use

Use the Calculate task when you need to:

- Calculate unit prices, totals, discounts or VAT amounts
- Convert units (cents to euros, grams to kilograms)
- Round numbers to a fixed number of decimals
- Pick the highest or lowest of several values
- Choose a value based on a comparison (`data.stock > 0 ? 1 : 0`)

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your data the calculations work on. Inside the expressions, `data` refers to this part.

- Use a simple column name (e.g., `order`)
- Access nested data using dot notation (e.g., `order.totals`)
- Leave **empty** to work with the root level of your data

If the key doesn't exist yet, it is created.

**Examples:**
- `order` - Calculate with and store results in the "order" object
- (empty) - Calculate on the top-level data

### Expressions

**Required** | **Type**: Grid | **Sortable**

Each row has three columns:

| Column | Description |
|---|---|
| **Column key/name** | Where the result is stored. Supports dot notation (`totals.vat`). |
| **Expression** | The calculation, see [Expression syntax](#expression-syntax). |
| **Decimals** | Optional. Rounds the result to this number of decimals. |

Rows are calculated **from top to bottom**, and every row can use the results of the rows above it.

## Expression syntax

Expressions are not tags: write them without `{{ }}`.

### Values

| Value | Example |
|---|---|
| Data (the selected key) | `data.total`, `data.customer.discount` |
| List items | `data.lines[0].qty` |
| Variables | `variables.vat_rate` |
| Cache | `cache.exchange_rates.EUR` |
| Numbers and text | `100`, `0.21`, `"big"` |

Numeric text such as `"12.50"` (common in API data) is treated as a number.

### Operators

| Operator | Meaning | Example |
|---|---|---|
| `+` `-` `*` `/` | Add, subtract, multiply, divide | `data.subtotal + data.shipping` |
| `%` | Remainder | `data.qty % 6` |
| `**` | Power | `data.side ** 2` |
| `==` `!=` `<` `>` `<=` `>=` | Compare | `data.total >= 50` |
| `and` `or` `not` | Combine conditions | `data.qty > 0 and data.price > 0` |
| `? :` | Choose a value | `data.total >= 50 ? 0 : 4.95` |
| `??` | Fallback for a missing field | `data.discount ?? 0` |

### Functions

`min`, `max`, `abs`, `round`, `floor`, `ceil`, `sqrt`, `pow`, `intdiv`, `fmod`

**Examples:** `max(data.price, 1)`, `round(data.total * 1.21, 2)`, `ceil(data.weight / 1000)`

## How It Works

1. The task selects the data at **Key / Column name** (or the whole data)
2. For each row, from top to bottom:
    - The expression is calculated with `data`, `variables` and `cache`
    - The result is rounded if **Decimals** is set
    - The result is stored at the row's **Column key/name**, so the next rows can use it
3. The updated data is passed on to the next task

**Error handling:**
- If a row fails (for example a typo, a missing field without `??`, or a division by zero), the task logs an error and skips that row. The other rows still run.
- If the selected key contains a single value instead of an object or list, the task logs an error and returns the data unchanged.

## Use Cases

### Unit Price per Order Line

**Scenario**: Your shop sends line totals, but the accounting system needs a unit price.

**Configuration:** inside a [Loop](loop.md) over `line_items`
- **Key / Column name**: (empty)
- **Expressions**:
    - `unit_price` = `data.subtotal / data.quantity`, decimals `2`

**Input (one line):**
```json
{ "name": "Bolt", "quantity": 2, "subtotal": "12.50" }
```

**Output:**
```json
{ "name": "Bolt", "quantity": 2, "subtotal": "12.50", "unit_price": 6.25 }
```

### Order Totals with VAT

**Scenario**: Calculate net amount, VAT and gross amount, using a VAT rate from the automation variables.

**Configuration:**
- **Key / Column name**: `order`
- **Expressions**:
    - `net` = `data.subtotal - (data.discount ?? 0)`, decimals `2`
    - `vat` = `data.net * variables.vat_rate`, decimals `2`
    - `gross` = `data.net + data.vat`, decimals `2`

**Input** (`vat_rate` = `0.21`):
```json
{ "order": { "subtotal": "100.00", "discount": "10.00" } }
```

**Output:**
```json
{ "order": { "subtotal": "100.00", "discount": "10.00", "net": 90, "vat": 18.9, "gross": 108.9 } }
```

### Free Shipping Threshold

**Configuration:**
- **Expressions**:
    - `shipping` = `data.total >= 50 ? 0 : 4.95`

### Convert Cents to Euros

**Configuration:**
- **Expressions**:
    - `price` = `data.price_cents / 100`, decimals `2`

## Best Practices

### Use `??` for Optional Fields

Reading a field that doesn't exist fails the row. Add a fallback for fields that are not always present:

- `data.discount ?? 0`
- `(data.shipping ?? 0) + data.subtotal`

### Order Your Rows

Rows run from top to bottom. Calculate intermediate values first (`net`), then the values that depend on them (`vat`, `gross`).

### Results Are Numbers

The result is a number, not text: `6.25`, not `"6.25"`. If the receiving system needs text or a fixed format such as `6.50`,
format the value afterwards with a [Set](set.md) task column, or with the receiving routine's schema.

### Preview with Real Data

API data is often text (`"12.50"`) and sometimes empty. Use the **Preview Function** with a real example to check the results and any errors.

## Related Tasks

- [Set](set.md) - Set or format values without calculations
- [Loop](loop.md) - Calculate per item in a list
- [Template](template.md) - Complex reshaping with Twig
