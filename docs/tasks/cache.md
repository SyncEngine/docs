# Cache Task

## Overview

The **Cache** task allows you to temporarily store, retrieve, and manage data during your automation workflow. Think of it as a temporary workspace where you can save values and access them later using the `{{ cache.your_tag }}` syntax.

## When to Use

Use the Cache task when you need to:

- Store intermediate results for use in later steps
- Keep track of running totals or counters across iterations
- Share data between different parts of your automation
- Build complex data structures across multiple steps
- Remember values from early steps for use much later
- Pass context between different branches of logic

## Settings

### Action

**Required** | **Type**: Select dropdown | **Default**: Set cache

Choose what you want to do with the cache:

- **Set cache**: Store a value in the cache
- **Get cache**: Retrieve a previously stored value and add it to your current data
- **Clear cache**: Remove a value from the cache

### Source

**Type**: Select dropdown | **Default**: Set from data | **Conditional**: Only when Action is "Set cache"

Choose where the value comes from:

- **Set from data**: Use data from the current step's output
- **Manual value**: Enter a specific value directly

### Key / Column name

**Type**: Text field | **Taggable**: Yes | **Conditional**: Not shown when Action is "Clear cache" and Source is "Manual value"

Specify which piece of data to work with:

- Use a simple key name (e.g., `customer_id`)
- Access nested data using dot notation (e.g., `customer.address.city`)
- Leave **empty** to work with all the data at root level

**Examples:**
- `order_total` - Work with the order_total field
- `customer.email` - Access nested customer email
- (empty) - Use all current data

### Cache value

**Type**: Text field | **Taggable**: Yes | **Conditional**: Only when Action is "Set cache" and Source is "Manual value"

Enter the value you want to store in the cache directly. Can include dynamic tags from previous steps.

**Examples:**
- `{{ previous.customer_id }}` - Store a value from a previous step
- `Complete` - Store a static text value
- `{{ data.price * 1.2 }}` - Store a calculated value

### Cache tag reference/path

**Required** | **Type**: Text field

The unique identifier for your cached value. This is the name you'll use to access the value later.

- Use descriptive names like `customer_total`, `current_page`, `api_token`
- Access later using `{{ cache.your_tag_name }}`
- Can include nested paths with dots (e.g., `customer.id`)

**Examples:**
- `order_count` - Access as `{{ cache.order_count }}`
- `api.token` - Access as `{{ cache.api.token }}`
- `temp_result` - Access as `{{ cache.temp_result }}`

### Method

**Type**: Select dropdown | **Default**: Replace | **Conditional**: Only when Action is "Set cache"

Choose how to handle the value if something is already cached with the same tag:

- **Replace**: Completely replace the old value with the new one
- **Merge**: Combine the new value with the old value (useful for objects/arrays)
- **Append**: Add the new value to the end of the existing value (useful for lists)

**When to use each:**
- **Replace**: Updating a counter, storing latest value
- **Merge**: Combining objects, adding new properties to existing data
- **Append**: Building lists, accumulating items across iterations

### Not found action

**Type**: Select dropdown | **Default**: Skip | **Conditional**: Only when Action is "Get cache"

What should happen if the cache tag doesn't exist:

- **Override with empty value**: Continue with an empty value
- **Skip task**: Don't execute this task at all

## How It Works

### Set Cache

1. **Determine Source Value**:
   - If **Source** is "Set from data": Retrieves value from the **Key / Column name** in current data
   - If **Source** is "Manual value": Uses the **Cache value** field directly

2. **Apply Method**:
   - **Replace**: Overwrites any existing cached value
   - **Merge**: Combines new value with existing (for arrays/objects)
   - **Append**: Adds new value to existing as an array item

3. **Store in Cache**: Saves to the **Cache tag reference/path** location

4. **Access Later**: Use `{{ cache.your_tag }}` in any subsequent task

### Get Cache

1. **Retrieve Value**: Looks up the value at **Cache tag reference/path**

2. **Handle Not Found**:
   - If tag doesn't exist and **Not found action** is "Skip": Task doesn't execute
   - If tag doesn't exist and **Not found action** is "Override": Uses empty value

3. **Merge into Data**: 
   - If **Key** is specified: Adds value to that key in current data
   - If **Key** is empty: Replaces entire data with cached value

### Clear Cache

1. **Remove Value**: Deletes the value at **Cache tag reference/path**
2. **Continue**: Data passes through unchanged

## Use Cases

### Store a Customer ID

**Scenario**: Save a customer ID early in the workflow for use in later steps.

**Configuration:**
- **Action**: Set cache
- **Source**: Set from data
- **Key**: `customer_id`
- **Cache tag reference**: `current_customer`

**Result**: Value accessible as `{{ cache.current_customer }}`

### Build a Running Total

**Scenario**: Accumulate values across a loop to calculate totals.

**Configuration:**
- **Action**: Set cache
- **Source**: Set from data
- **Key**: `order_amount`
- **Cache tag reference**: `all_amounts`
- **Method**: Append

**Result**: Each iteration adds to the list in `{{ cache.all_amounts }}`

### Retrieve Stored Value

**Scenario**: Bring a previously cached value back into the data flow.

**Configuration:**
- **Action**: Get cache
- **Cache tag reference**: `current_customer`
- **Key**: `customer_id`
- **Not found action**: Skip task

**Result**: Cached value is added to data at the `customer_id` key

### Count Loop Iterations

**Scenario**: Track how many times a loop has run.

**Loop start:**
- **Action**: Set cache
- **Source**: Manual value
- **Cache value**: `0`
- **Cache tag reference**: `counter`

**Each iteration:**
- **Action**: Set cache
- **Source**: Manual value
- **Cache value**: `{{ cache.counter + 1 }}`
- **Cache tag reference**: `counter`
- **Method**: Replace

### Merge Object Properties

**Scenario**: Build up a complex object across multiple steps.

**Configuration:**
- **Action**: Set cache
- **Source**: Set from data
- **Cache tag reference**: `customer_profile`
- **Method**: Merge

**Result**: Each execution adds/updates properties in the cached object

## Best Practices

**Use descriptive tag names**: Make it clear what's stored - `customer_email` is better than `temp1`.

**Clear cache when done**: If you're done with a cached value, clear it to avoid confusion in later steps.

**Choose the right method**: 
- Use Replace for simple updates
- Use Append for building lists
- Use Merge for combining objects

**Remember cache is temporary**: Cache only exists during the automation run. Use Storage for persistent data.

**Avoid tag name conflicts**: Use unique, specific names to prevent accidentally overwriting cached values.

**Use nested paths for organization**: Group related cache values like `api.token`, `api.endpoint`, `api.response`.

## Notes

- Cache is temporary and only exists during the current automation run
- Cache is shared across all steps in the automation
- You can store simple values (text, numbers) or complex data (lists, objects)
- Cache tags are case-sensitive
- The Merge method only works with arrays and objects, not scalar values
- Append always creates or adds to an array
- Use Storage task for persistent data that needs to survive across automation runs
- Cache values are accessible in conditions, tags, and all task fields that support tagging
