# Choose Task

## Overview

The **Choose** task allows you to create branching logic in your automation workflows. It evaluates multiple options with conditions and executes the tasks associated with the first matching option. Think of it as an "if-else if-else" statement that lets your automation take different paths based on your data.

## When to Use

Use the Choose task when you need to:

- Route data to different actions based on specific criteria
- Execute different tasks depending on data values
- Create decision trees with multiple possible paths
- Handle different scenarios with unique processing requirements
- Implement business logic with conditional branches
- Process customers, orders, or records differently based on their properties

## Settings

### Options

**Type**: Repeater field

Define multiple conditional branches that are evaluated in sequence. Only the **first matching option** will execute its tasks, then the Choose task completes.

Each option includes:

#### Conditions

**Required** | **Type**: Conditions field | **Taggable**: Yes

Set up conditions that determine when this option should be selected. The Choose task evaluates these conditions against your current data.

**Source options:**
- **Data** (default): Check against data from previous steps
- **Cache**: Compare values using `{{ cache.key }}`
- **Variables**: Evaluate against `{{ variables.name }}`
- **Customizable**: Reference any data point using tags

**Example conditions:**
- `{{ data.order.total }}` is greater than `1000`
- `{{ cache.customer_type }}` equals `"premium"`
- `{{ data.status }}` contains `"pending"`
- `{{ variables.region }}` equals `"US"`

#### Tasks

**Required** | **Type**: Task list

Add one or more tasks that will run when this option's conditions are met. Once these tasks complete, the Choose task ends - remaining options are not evaluated.

**Option features:**
- **Label**: Give the option a descriptive name (shown in execution logs)
- **Disable**: Temporarily disable an option without deleting it
- **Delete**: Remove an option permanently

### Default Tasks

**Type**: Task list | **Optional** | **Collapsed by default**

Define fallback tasks that execute if **none** of the option conditions match. This acts as the "else" clause in your logic.

If no options match and no default tasks are defined, the Choose task passes data through unchanged.

## How It Works

1. **Evaluate Options**: The Choose task evaluates each option sequentially from top to bottom

2. **For Each Option**:
   - Skips disabled options
   - Checks if conditions are met using current data
   - When a match is found:
     - Executes the tasks defined for that option
     - Logs which option was selected (by index and label)
     - **Stops** checking remaining options

3. **Default Fallback**: If no options match:
   - Executes default tasks (if defined)
   - Logs that the default path was taken

4. **Return Data**: Returns the data after executing the selected option's tasks

**Important**: Only the first matching option runs. Once matched, all other options are ignored.

## Use Cases

### Customer Tier Processing

Route customers to different processes based on their account value.

**Option 1**: Premium Tier
- **Conditions**: `{{ data.order_total }}` is greater than `5000`
- **Tasks**: VIP processing workflow

**Option 2**: Standard Tier
- **Conditions**: `{{ data.order_total }}` is greater than `1000`
- **Tasks**: Standard processing workflow

**Default Tasks**: Basic processing for new customers

### Order Status Handling

Take different actions based on order status.

**Option 1**: Pending Orders
- **Conditions**: `{{ data.status }}` equals `"pending"`
- **Tasks**: Send reminder email

**Option 2**: Shipped Orders
- **Conditions**: `{{ data.status }}` equals `"shipped"`
- **Tasks**: Update tracking information

**Option 3**: Delivered Orders
- **Conditions**: `{{ data.status }}` equals `"delivered"`
- **Tasks**: Request customer feedback

**Default Tasks**: Log unexpected status

### Multi-Region Processing

Process data differently based on location.

**Option 1**: EU Region
- **Conditions**: `{{ data.region }}` equals `"EU"`
- **Tasks**: Apply GDPR compliance tasks

**Option 2**: US Region
- **Conditions**: `{{ data.region }}` equals `"US"`
- **Tasks**: Apply US-specific regulations

**Option 3**: APAC Region
- **Conditions**: `{{ data.region }}` equals `"APAC"`
- **Tasks**: Apply APAC workflows

**Default Tasks**: Standard international processing

### Priority-Based Routing

Handle requests differently based on priority level.

**Option 1**: Critical Priority
- **Conditions**: 
  - `{{ data.priority }}` equals `"critical"` AND
  - `{{ data.response_time }}` is less than `5`
- **Tasks**: Immediate alert + escalation

**Option 2**: High Priority
- **Conditions**: `{{ data.priority }}` equals `"high"`
- **Tasks**: Priority queue processing

**Default Tasks**: Standard queue processing

## Best Practices

**Order Options Strategically**: Place more specific conditions before general ones. The first match wins, so order matters.

✅ **Good ordering:**
1. Premium AND high value → Special VIP handling
2. Premium → Standard premium handling  
3. Default → Regular handling

❌ **Poor ordering:**
1. Premium → Standard premium handling (too broad)
2. Premium AND high value → Never reached!

**Use Descriptive Labels**: Name your options clearly so execution logs are easy to understand:
- "Premium Customer Processing"
- "EU Region Compliance"  
- "Critical Priority Alert"

**Leverage Default Tasks**: Always consider a default fallback to handle unexpected cases gracefully.

**Test Edge Cases**: Verify your conditions handle edge cases like empty values, missing fields, or unexpected data types.

**Keep Conditions Simple**: If an option has complex logic, consider breaking it into multiple options or simplifying the condition structure.

**Document Your Logic**: Use option labels to document the business logic, making workflows easier to maintain.

## Notes

- Options are evaluated **sequentially** - order determines which option runs
- Only **one option** ever executes (the first match)
- Disabled options are completely skipped during evaluation
- The execution log shows which option was selected (index and label)
- If an option's conditions match but it has no tasks, the Choose task still considers it selected (no further options evaluated)
- Conditions support full validation logic: AND, OR, nested groups, multiple conditions
- Dynamic tags in conditions are evaluated before checking
- The data available to option tasks includes all data from before the Choose task
- Variables and cache values are accessible in option conditions and tasks