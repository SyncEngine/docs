# Loop Task

## Overview

The **Loop** task allows you to iterate over a list of items and execute actions for each item (or batch of items). It's the primary way to process multiple records individually, running the same set of tasks, flow, or routine for each one. Think of it as a "for each" loop in programming, or similar to looping actions in Zapier or Make.

## When to Use

Use the Loop task when you need to:

- Process each item in a list individually
- Run the same workflow for multiple records
- Execute a flow or routine for each row of data
- Handle bulk operations one item at a time
- Process data in batches for better performance
- Apply transformations to each item in a collection
- Send individual notifications or API calls for each record

## Settings

### Key / Column name

**Type**: Text field | **Optional** | **Taggable**: Yes

Specifies which part of your data contains the list you want to iterate over.

- Use a simple column name (e.g., `customers`)
- Access nested data using dot notation (e.g., `response.data.items`)
- Leave **empty** to iterate over the root level of your data (the entire data set)

**Examples:**
- `orders` - Loop through the "orders" array
- `api.results` - Loop through items in a nested "results" array
- (empty) - Loop through items at the top level

### Loop method

**Type**: Select dropdown | **Default**: Row

Choose how items are processed:

- **Row**: Process items one at a time (each item individually)
- **Batches**: Process items in groups of a specified size

**When to use each method:**

- Use **Row** for most cases where you need to process each item separately
- Use **Batches** when processing large datasets or when your actions work better with multiple items at once (e.g., bulk API calls)

### Batch size

**Type**: Number field | **Required** when Loop method is "Batches" | **Conditional**: Only appears when Loop method is "Batches"

The number of items to process together in each batch.

**Examples:**
- `10` - Process 10 items at a time
- `50` - Process 50 items at a time
- `100` - Process 100 items at a time

**Considerations:**
- Larger batches = fewer iterations but more data per iteration
- Smaller batches = more iterations but less memory usage
- Choose based on your action's requirements and performance needs

### Action

**Required** | **Type**: Select dropdown

Choose what to execute for each item (or batch):

- **Flow**: Run a complete flow for each item
- **Routine**: Execute a routine for each item
- **Tasks**: Run a set of tasks for each item

**When to use each action type:**

- **Flow**: When you have a reusable workflow that needs to run for each item
- **Routine**: For scheduled or complex processes that should run per item
- **Tasks**: For simple, inline processing without creating a separate flow

### Flow

**Type**: Entity selector | **Conditional**: Only appears when Action is "Flow"

Select which flow to execute for each item. You can:
- Select an existing flow
- Edit the selected flow
- Create a new flow

The selected flow will receive each item as its input data.

### Routine

**Type**: Entity selector | **Conditional**: Only appears when Action is "Routine"

Select which routine to execute for each item. You can:
- Select an existing routine
- Edit the selected routine
- Create a new routine

The selected routine will receive each item as its input data.

### Tasks

**Type**: Tasks field | **Conditional**: Only appears when Action is "Tasks"

Define the tasks to execute for each item. Build your processing logic inline without creating a separate flow or routine.

## How It Works

### Row Method (Individual Processing)

1. Locates the data specified in **Key / Column name**
2. Verifies that the data is iterable (a list or array)
3. For each item in the list:
    - Sets up the loop context with current item data
    - Makes the item available as `{{ context.loop.data }}`
    - Provides iteration tracking via `{{ context.loop.iteration }}`
    - Executes the selected action (Flow, Routine, or Tasks) with the current item
    - Updates progress tracking for monitoring
    - Stores the result back to the same position in the list
4. Replaces the original data with the processed results
5. Cleans up the loop context

### Batch Method (Grouped Processing)

1. Locates the data specified in **Key / Column name**
2. Verifies that the data is iterable (a list or array)
3. Splits the items into batches of the specified size
4. For each batch:
    - Sets up the loop context with current batch data
    - Makes the batch available as `{{ context.loop.data }}`
    - Provides batch tracking via `{{ context.loop.iteration }}`
    - Executes the selected action with the entire batch
    - Updates progress tracking
    - Merges the processed batch back into the original list
5. Replaces the original data with all processed results
6. Cleans up the loop context

**Error handling:**
- If the specified key is not found, an error is logged and data returns unchanged
- If the data is not iterable, an error is logged and data returns unchanged
- If no valid action is selected, an error is logged
- For batch method without batch size, an error is logged

## Use Cases

### Send Email to Each Customer

**Scenario**: Send a personalized email to each customer in a list.

**Configuration:**
- **Key / Column name**: `customers`
- **Loop method**: Row
- **Action**: Tasks
- **Tasks**: Email task using `{{ context.loop.data.email }}` and `{{ context.loop.data.name }}`

**Result**: Each customer receives an individual email with their specific information.

### Process Orders in Batches

**Scenario**: Submit orders to an API that accepts bulk requests (up to 25 orders at once).

**Configuration:**
- **Key / Column name**: `orders`
- **Loop method**: Batches
- **Batch size**: `25`
- **Action**: Tasks
- **Tasks**: API request with `{{ context.loop.data }}` (contains 25 orders)

**Result**: Orders are processed in groups of 25, reducing API calls.

### Run Flow for Each Product

**Scenario**: Execute a product enrichment flow for each product in your catalog.

**Configuration:**
- **Key / Column name**: `products`
- **Loop method**: Row
- **Action**: Flow
- **Flow**: "Product Enrichment Flow"

**Result**: The enrichment flow runs once for each product, with each product as the flow's input.

### Generate Individual Reports

**Scenario**: Create a PDF report for each sales transaction.

**Configuration:**
- **Key / Column name**: `transactions`
- **Loop method**: Row
- **Action**: Tasks
- **Tasks**: 
    1. Format data for report
    2. Generate PDF
    3. Save to storage

**Result**: Individual PDF report created for each transaction.

### Process Large Dataset Efficiently

**Scenario**: Transform 10,000 records but avoid memory issues by processing in chunks.

**Configuration:**
- **Key / Column name**: `records`
- **Loop method**: Batches
- **Batch size**: `100`
- **Action**: Tasks
- **Tasks**: Transformation tasks

**Result**: Records processed 100 at a time, managing memory efficiently.

## Available Data in Loop Context

Within the tasks, flow, or routine being executed for each iteration, you have access to special loop context data:

### For Row Method

- `{{ context.loop.data }}` - The current item being processed
- `{{ context.loop.data.fieldname }}` - Access specific fields from the current item
- `{{ context.loop.index }}` - The original index/key of the current item
- `{{ context.loop.iteration.current }}` - The current iteration number (1, 2, 3, ...)
- `{{ context.loop.iteration.index }}` - The current iteration index (0, 1, 2, ...)

### For Batch Method

- `{{ context.loop.data }}` - The current batch (array of items)
- `{{ context.loop.index }}` - The batch number (0, 1, 2, ...)
- `{{ context.loop.iteration.current }}` - The current batch number (1, 2, 3, ...)
- `{{ context.loop.iteration.index }}` - The current batch index (0, 1, 2, ...)

### Example Usage

```
Send email to {{ context.loop.data.email }}
Processing item {{ context.loop.iteration.current }} of {{ context.loop.iteration.total }}
Current customer: {{ context.loop.data.name }}
```

## Row Method vs. Batch Method

### Row Method (Individual)

**Best for:**
- Individual API calls or emails
- When each item needs isolated processing
- When order of processing matters
- Simpler logic and debugging

**Performance:**
- More iterations
- Lower memory per iteration
- Better for real-time feedback

### Batch Method (Groups)

**Best for:**
- Bulk API operations
- Database batch inserts
- When processing many items is more efficient together
- Large datasets where individual processing is too slow

**Performance:**
- Fewer iterations
- Higher memory per iteration
- Better throughput for large datasets

## Best Practices

### Choose the Right Loop Method

- Default to **Row** method for clarity and simplicity
- Use **Batches** only when you have a specific performance need or your action supports bulk operations

### Keep Actions Focused

Make your loop actions efficient:
- ✅ Process only what's necessary for each item
- ❌ Avoid heavy operations that don't need to run per item
- ✅ Use batch method for bulk operations when possible

### Monitor Performance

- Watch execution time for loops with many items
- Consider batch processing for lists over 100-200 items
- Test with sample data first

### Handle Errors Gracefully

- Use the Attempt task inside loops to handle individual failures
- Consider logging failed items for later review
- Don't let one failed item stop the entire loop

### Use Meaningful Data Access

Be clear about what data you're accessing:
- `{{ context.loop.data }}` - The current item
- `{{ context.loop.data.specific_field }}` - A specific field from the current item
- `{{ context.loop.index }}` - The item's position

### Consider Memory Limits

For very large datasets:
- Use batch processing to manage memory
- Process in smaller chunks
- Consider pagination if data comes from an API

## Understanding Progress Tracking

The Loop task automatically tracks progress:

- **Total**: Number of items (or batches)
- **Current**: Which item (or batch) is being processed
- **Progress**: Visual indicator in execution traces

This helps you:
- Monitor long-running loops
- Estimate completion time
- Debug which iteration caused issues

## Common Patterns

### Loop with Conditional Processing

1. **Loop** through items
2. Inside loop: **Choose** task to branch based on item data
3. Process differently based on conditions

### Loop with Error Handling

1. **Loop** through items
2. Inside loop: **Attempt** task wrapping the main logic
3. Catch errors and continue to next item

### Nested Loops

1. **Loop** through categories
2. Inside first loop: **Loop** through items in each category
3. Process each item with context from both loops

### Loop with Aggregation

1. **Loop** through items with transformations
2. After loop: Use results for aggregation (sum, count, etc.)
3. Generate summary report

## Notes

- The Loop task processes items in order (sequential, not parallel)
- Original array keys/indexes are preserved in the output
- For batch processing, the last batch may be smaller if items don't divide evenly
- Loop context data is only available inside the loop's action
- The loop data is replaced in-place at the specified key location
- Progress tracking is automatically updated during execution
- Nested loops are supported (loop within a loop)
- The loop context is cleaned up after the loop completes
- You can access both the loop data and the original workflow data inside the loop
- Dynamic tags are supported in the **Key / Column name** field
- For batch method, array keys are preserved when merging results back
