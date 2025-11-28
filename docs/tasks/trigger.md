# Trigger Task

## Overview

The **Trigger** task allows you to execute other automations, flows, routines, or task sequences from within your current workflow. Think of it as a way to call reusable workflows, break complex automations into manageable pieces, or execute independent processes without interrupting your main flow.

## When to Use

Use the Trigger task when you need to:

- Execute another automation from within your workflow
- Reuse flows or routines across multiple automations
- Break complex automations into smaller, maintainable pieces
- Run independent processes without blocking your main workflow (async)
- Execute tasks with different data contexts
- Chain automations together
- Call utility workflows for common operations

## Settings

### Run async? (Experimental)

_**Experimental**: Passing variables and data to async trigger is not yet supported!_

**Type**: Switch (Yes/No) | **Default**: No | **Conditional**: Only when Action is "Automation" and Override data is off

Run the triggered automation asynchronously (in the background) without waiting for it to complete.

- **No (off)**: Wait for the triggered automation to finish before continuing
- **Yes (on)**: Schedule the automation to run in background, continue immediately

**Note**: Automations with batch processing always run async regardless of this setting.

### Pass current data?

**Type**: Switch (Yes/No) | **Default**: No

Control whether to send data to the triggered workflow.

- **No**: Triggered workflow starts with empty data
- **Yes**: Sends data from current workflow (see Key setting)

### Override current data?

**Type**: Switch (Yes/No) | **Default**: No | **Conditional**: Only when Run async is off

Control whether the triggered workflow's output replaces your current data.

- **No**: Triggered workflow runs, but your data stays unchanged
- **Yes**: Result from triggered workflow replaces your current data

**Note**: Only available for synchronous triggers (async triggers can't return data).

### Key / Column name

**Type**: Text field | **Taggable**: Yes | **Conditional**: Only when Pass current data is on

Specify which data to pass to the triggered workflow.

- Use a simple column name (e.g., `customer`)
- Access nested data using dot notation (e.g., `order.customer`)
- Leave **empty** to pass all root-level data

**For passing data**: Data at this key is sent to the triggered workflow
**For receiving data**: Result is placed at this key (when Override is on)

**Examples:**
- `customer` - Pass/receive the customer object
- `order.items` - Pass nested order items
- (empty) - Pass/receive all data

### Variables

**Type**: Key-value pairs (params field) | **Taggable**: Yes | **Collapsed by default**

Define static variables to be used by the triggered workflow. These are merged with any existing variables from the current workflow.

**Examples:**
- `api_endpoint` = `"https://api.example.com"`
- `environment` = `"production"`
- `customer_id` = `{{ data.customer.id }}`

Use in triggered workflow as `{{ variables.api_endpoint }}`

### Action

**Required** | **Type**: Select dropdown

Choose what type of workflow to trigger:

- **Automation**: Trigger a complete automation (with connections, batching, etc.)
- **Flow**: Execute a reusable flow
- **Routine**: Execute a routine workflow
- **Tasks**: Execute a custom sequence of tasks defined in this trigger

### Automation

**Type**: Entity selector | **Conditional**: Only when Action is "Automation"

Select which automation to trigger. You can edit or create automations directly from this field.

### Flow

**Type**: Entity selector | **Conditional**: Only when Action is "Flow"

Select which flow to execute. Flows are reusable task sequences that can be shared across multiple automations.

### Routine

**Type**: Entity selector | **Conditional**: Only when Action is "Routine"

Select which routine to execute. Routines are scheduled workflows that can also be triggered manually.

### Tasks

**Type**: Task list | **Conditional**: Only when Action is "Tasks"

Define a custom sequence of tasks to execute directly within this trigger. This lets you create inline workflows without setting up separate flows.

## How It Works

### Synchronous Execution (Async Off)

1. **Validate Configuration**: Ensures action type and target are configured

2. **Prepare Context**: Creates a new execution context with:
   - Passed variables (merged with existing)
   - Parent context reference (for nested execution)

3. **Prepare Data**:
   - If **Pass data** is on: Extracts data from Key (or all data)
   - If **Pass data** is off: Starts with empty data

4. **Execute Workflow**: Runs the selected automation/flow/routine/tasks synchronously (waits for completion)

5. **Handle Result**:
   - If **Override data** is on: Replaces current data at Key (or root)
   - If **Override data** is off: Discards result, keeps original data

6. **Return to Parent**: Execution returns to the parent workflow

### Asynchronous Execution (Async On)

1. **Schedule Automation**: Queues the automation to run in the background

2. **Continue Immediately**: Returns control to parent workflow without waiting

3. **No Data Return**: Async automations cannot return data to the parent

## Use Cases

### Call Reusable Flow

**Scenario**: Execute a common email notification flow used across multiple automations.

**Configuration:**
- **Action**: Flow
- **Flow**: `send_notification_email`
- **Pass current data**: Yes
- **Key**: `notification_data`

Result: The notification flow runs with the notification data, then returns to the main workflow.

### Chain Automations

**Scenario**: After processing an order, trigger a separate inventory update automation.

**Configuration:**
- **Action**: Automation
- **Automation**: `update_inventory`
- **Run async**: Yes
- **Pass current data**: Yes
- **Key**: `order.items`

Result: Inventory automation runs in background while order processing continues.

### Execute Inline Tasks

**Scenario**: Perform a quick transformation without creating a separate flow.

**Configuration:**
- **Action**: Tasks
- **Tasks**: 
  - Set: Calculate discount
  - Set: Apply tax
- **Pass current data**: Yes
- **Override current data**: Yes

Result: Inline tasks execute and return modified data to the main workflow.

### Run Routine Manually

**Scenario**: Trigger a scheduled routine from within an automation.

**Configuration:**
- **Action**: Routine
- **Routine**: `daily_cleanup`
- **Pass current data**: No

Result: The cleanup routine executes immediately (not waiting for its schedule).

### Pass Variables to Triggered Workflow

**Scenario**: Execute an automation with specific configuration.

**Configuration:**
- **Action**: Automation
- **Automation**: `process_customer`
- **Variables**:
  - `mode` = `"test"`
  - `notify` = `false`
- **Pass current data**: Yes

Result: Triggered automation has access to `{{ variables.mode }}` and `{{ variables.notify }}`.

### Get Result from Flow

**Scenario**: Execute a calculation flow and use its result.

**Configuration:**
- **Action**: Flow
- **Flow**: `calculate_shipping_cost`
- **Pass current data**: Yes
- **Key**: `order`
- **Override current data**: Yes

Result: Shipping cost calculation runs and replaces the order data with updated information.

## Trigger Actions

### Automation

Triggers a full automation with all its features:
- Connections (source data, send results)
- Batch processing
- Iterator configuration
- Complete automation lifecycle

**Use for**: Independent processes, scheduled automations, complex workflows with connections.

**Note**: Automations with batching always run async.

### Flow

Executes a reusable flow (sequence of tasks).

**Use for**: Shared logic, common task sequences, modular workflow components.

### Routine

Triggers a routine workflow.

**Use for**: Scheduled tasks, maintenance workflows, periodic operations triggered on-demand.

### Tasks

Executes an inline sequence of tasks defined directly in the trigger.

**Use for**: Quick transformations, simple operations, avoiding separate flow creation.

## Async vs Sync

### Synchronous (Async Off)

**Behavior**: Waits for triggered workflow to complete before continuing.

**Pros:**
- Can return data to parent workflow
- Sequential execution (predictable order)
- Errors propagate to parent

**Cons:**
- Blocks parent workflow
- Slower for independent operations

**Use when**: Need result data, must wait for completion, sequential processing required.

### Asynchronous (Async On)

**Behavior**: Schedules workflow to run in background, continues immediately.

**Pros:**
- Parent workflow doesn't wait
- Parallel processing
- Better performance for independent tasks

**Cons:**
- Cannot return data
- Errors don't propagate to parent
- Less predictable timing

**Use when**: Independent operations, fire-and-forget tasks, parallel processing.

## Pass Data Patterns

### No Data Passed

```
Trigger runs with empty data
```

Use when: Triggered workflow doesn't need parent data, has its own data source.

### Specific Key Passed

```
Parent: {customer: {...}, order: {...}}
Key: "customer"
→ Triggered workflow gets: {customer: {...}}
```

Use when: Triggered workflow needs subset of parent data.

### All Data Passed

```
Parent: {customer: {...}, order: {...}}
Key: (empty)
→ Triggered workflow gets: {customer: {...}, order: {...}}
```

Use when: Triggered workflow needs complete parent context.

### Result Overrides

```
Triggered result: {total: 150, tax: 15}
Override: Yes, Key: "order"
→ Parent data.order becomes: {total: 150, tax: 15}
```

Use when: Need calculated result from triggered workflow.

## Best Practices

**Choose the Right Action Type**: Use Flows and Routines for reusable logic, Automations for complete independent processes, Tasks for simple inline operations.

**Use Async for Independent Operations**: If the triggered workflow doesn't need to return data and doesn't block the parent, run it async for better performance.

**Pass Only Necessary Data**: Don't pass entire data sets when triggered workflow only needs specific fields. Use the Key setting to be selective.

**Leverage Variables**: Pass configuration and context through variables rather than embedding in data.

**Avoid Deep Nesting**: Too many nested triggers can become hard to debug. Consider flattening workflows or using different automation triggers.

**Handle Errors**: Remember that async triggers don't propagate errors to the parent. Implement error handling in the triggered workflow.

**Document Trigger Purpose**: Use clear names for flows/routines and add comments in inline tasks to explain what's being triggered and why.

## Notes

- Synchronous triggers can return data, asynchronous cannot
- Automations with batch/iterator processing always run async
- Variables from parent workflow are inherited by triggered workflow
- Triggered workflows have their own execution context
- Nested triggers are supported (triggers within triggers)
- Async triggers are queued for execution but timing is not guaranteed
- Override data only works with synchronous triggers
- Empty data can be passed (null, empty arrays, empty objects)
- The parent workflow's cache is not shared with triggered workflows
- Error handling in async triggers must be done within the triggered workflow
- Triggered automations can access their own connections independently