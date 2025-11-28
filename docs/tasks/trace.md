# Trace Task

## Overview

The **Trace** task allows you to log messages and errors during automation execution. Use it to track progress, debug issues, record important events, or intentionally mark an automation as failed. Trace logs appear in the automation's execution history.

## When to Use

Use the Trace task when you need to:

- Log important events or milestones in your automation
- Debug workflows by recording data values at specific points
- Create audit trails of automation actions
- Mark an automation as failed intentionally (error traces)
- Record warnings or notices for review
- Track which path was taken in complex logic
- Document processing status for later analysis

## Settings

### Trace type

**Required** | **Type**: Select dropdown

Choose the severity level of the trace:

- **Log message**: Creates an informational log entry (automation continues, marked as successful)
- **Error message**: Creates an error entry and marks the automation as failed

**When to use each:**
- **Log**: Track progress, record events, debug information
- **Error**: Intentionally fail the automation, flag critical issues

### Message

**Required** | **Type**: Text field | **Taggable**: Yes

The main message to log. This appears as the primary text in the execution history.

**Examples:**
- `"Customer processed successfully"`
- `"Processing {{ data.customer_count }} customers"`
- `"Critical: Payment threshold exceeded"`
- `"Debug: Entering premium customer workflow"`

### Extra info

**Type**: Text field | **Taggable**: Yes

Additional details or context to include with the trace. This appears as secondary information in the log.

**Examples:**
- `"Customer ID: {{ data.customer_id }}"`
- `"Order total: ${{ data.order_total }}"`
- Additional metadata or debugging details

### Include data?

**Type**: Select dropdown | **Customizable** | **Taggable**: Yes | **Default**: No

Choose whether to include workflow data in the trace log:

- **No**: Don't include any data
- **Yes**: Include all workflow data (select the `.` option)
- **Custom key**: Include data from a specific key/path

When enabled, the data is attached to the trace log entry for inspection.

**Examples:**
- (empty) - No data included
- `.` - Include all current data
- `customer` - Include only the customer object
- `order.items` - Include only the order items array

## How It Works

1. **Validate Message**: Ensures a message is configured (errors if missing)

2. **Prepare Trace Data**:
   - Gets the message and trace type
   - Retrieves extra info (if provided)
   - Extracts workflow data to include (if configured)

3. **Create Trace Log**: Constructs a trace log entry with:
   - Message text
   - Trace type (log or error)
   - Execution context (automation, flow, routine info)
   - Extra info (if provided)
   - Workflow data snapshot (if included)

4. **Record Trace**:
   - **Error type**: Adds to context as error (marks automation as failed)
   - **Log type**: Adds to context as log (informational only)

5. **Continue**: The automation continues executing (data unchanged)

## Use Cases

### Track Processing Progress

**Scenario**: Log milestones as data moves through the automation.

**Configuration:**
- **Trace type**: Log message
- **Message**: `"Processed {{ data.customer_count }} customers"`
- **Include data**: No

Result: Creates a log entry showing how many customers were processed.

### Debug Data Flow

**Scenario**: Inspect data at a specific point in the automation.

**Configuration:**
- **Trace type**: Log message
- **Message**: `"Data checkpoint at step 5"`
- **Include data**: Yes (`.`)

Result: Logs the complete data state for debugging.

### Record Critical Events

**Scenario**: Log when important thresholds are crossed.

**Configuration:**
- **Trace type**: Log message
- **Message**: `"High-value order detected"`
- **Extra info**: `"Order ID: {{ data.order_id }}, Total: ${{ data.total }}"`
- **Include data**: `order`

Result: Detailed log of the high-value order for audit purposes.

### Intentional Failure

**Scenario**: Fail the automation when data doesn't meet requirements.

**Configuration:**
- **Trace type**: Error message
- **Message**: `"Invalid data: Missing required customer email"`
- **Include data**: `customer`

Result: Automation marked as failed with error details for review.

### Path Tracking

**Scenario**: Record which branch of logic was executed.

**Configuration:**
- **Trace type**: Log message
- **Message**: `"Entered premium customer workflow"`

Result: Execution history shows which path was taken.

### Log API Response

**Scenario**: Record API responses for troubleshooting.

**Configuration:**
- **Trace type**: Log message
- **Message**: `"API call completed"`
- **Extra info**: `"Status: {{ data.response.status }}"`
- **Include data**: `response`

Result: Full API response logged for later analysis.

## Trace Types

### Log Message (Informational)

Creates a log entry without affecting automation success status.

**Use for:**
- Progress tracking
- Debug information
- Audit trails
- Event recording
- Performance milestones

**Result**: Automation continues and completes successfully (unless other errors occur).

### Error Message (Failure)

Creates an error entry and marks the automation run as failed.

**Use for:**
- Data validation failures
- Critical condition violations
- Intentional workflow termination
- Unrecoverable situations

**Result**: Automation marked as failed in execution history.

## Best Practices

**Use Descriptive Messages**: Make messages clear and searchable:
- ✅ `"Customer email validation failed"`
- ❌ `"Error"`

**Include Context**: Use tags to add specific details:
- `"Processing order {{ data.order_id }}"`
- `"Customer count: {{ data.customers|length }}"`

**Use Extra Info Strategically**: Put detailed information in the extra info field to keep messages concise but informative.

**Log Key Decision Points**: Add trace logs at important branching points to understand execution flow later.

**Include Data for Debugging**: When troubleshooting, include relevant data snapshots to see exactly what the automation was working with.

**Use Errors Intentionally**: Only use error traces when you truly want to mark the automation as failed. For warnings, use log messages.

**Consider Log Volume**: Too many trace logs can make execution history noisy. Log strategically at key points rather than every step.

**Structure Extra Info**: Use consistent formatting in extra info for easier parsing later.

## Include Data Options

### No Data

Trace contains only the message and extra info.

**Use when**: Message alone is sufficient, or data is sensitive.

### Full Data (`.`)

Includes the entire workflow data state.

**Use when**: Debugging complex issues, need complete context.

### Specific Key

Includes only data from a specific key path.

**Examples:**
- `customer` - Include only customer object
- `order.items` - Include only order items
- `response.body` - Include API response body

**Use when**: Need relevant context without logging everything.

## Notes

- Trace logs appear in the automation execution history
- Error traces mark the automation run as failed
- Log traces do not affect automation success status
- Workflow data is not modified by the Trace task
- Complex data structures (arrays, objects) are automatically normalized for logging
- Trace logs persist in execution history for review
- The execution context (automation, flow, routine info) is automatically included
- Dynamic tags in messages and extra info are evaluated before logging
- Messages and extra info can include any SyncEngine tags
- Include data uses dot notation for nested key paths
- Empty or missing keys in included data result in null values
- Very large data sets can make logs harder to read - be selective with included data