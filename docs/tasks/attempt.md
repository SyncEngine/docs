# Attempt Task

## Overview

The **Attempt** task allows you to execute actions while gracefully handling any errors that may occur. Think of it as a "try-catch" block for your automation workflows - it lets you try risky operations and define backup plans when things go wrong.

## When to Use

Use the Attempt task when you need to:

- Handle potential API failures with fallback options
- Try primary actions with backup alternatives
- Gracefully handle missing or invalid data
- Continue workflows even when non-critical actions fail
- Implement error-specific handling logic
- Log errors without stopping the entire automation

## Settings

### Actions

**Required** | **Type**: Task list

The main actions you want to attempt to execute. Add one or more tasks that will run normally. If any of these actions encounters an error, it will trigger the error handling defined in the **Catch errors** section.

**Examples:**
- Call an external API
- Process customer data
- Send notifications
- Update database records

### Catch Errors

**Type**: Repeater field

Define how to handle errors when they occur. You can create multiple catch rules that are evaluated in order. The first matching rule will handle the error.

Each catch rule contains:

#### Conditions

**Optional** | Define which errors this rule should catch.

- Leave **empty** to catch all errors
- Use conditions to catch specific error types
- Access error details using the `{{ error }}` placeholder

**Available error data:**
- `{{ error.message }}` - The error message text
- `{{ error.type }}` - The error type/category
- `{{ error.response }}` - Response data (for API errors)
- Additional fields depending on the error source

**Example conditions:**
- `{{ error.message }}` contains `"timeout"` - Catch timeout errors
- `{{ error.response.info.http_code }}` equals `404` - Catch 404 errors
- `{{ error.type }}` equals `"connection"` - Catch connection errors

#### Actions

**Optional** | **Type**: Task list

The tasks to run when this catch rule is triggered. Define fallback actions, logging, notifications, or alternative processing.

**Examples:**
- Use a backup API endpoint
- Send error notifications
- Set default values
- Log to error tracking system

## How It Works

1. **Execute Actions**: The task runs all actions defined in the **Actions** field sequentially

2. **Error Detection**: If any action encounters an error:
   - Execution of remaining actions in the Actions list stops
   - The error is captured and passed to the catch handlers
   - The `{{ error }}` variable becomes available with error details

3. **Catch Evaluation**: Each **Catch errors** rule is evaluated in order:
   - If conditions match (or no conditions exist), the catch rule triggers
   - The catch rule's actions execute
   - Evaluation stops after the first matching rule

4. **Error Logging**: 
   - Caught errors are logged as notices (not failures)
   - Uncaught errors are logged as errors
   - The automation continues running

5. **Data Flow**: The automation continues with the data state from before the error occurred

## Use Cases

### API Fallback

Try a primary API, fall back to secondary if it fails.

**Actions:**
- Retrieve: Call primary API endpoint

**Catch errors:**
- Conditions: (empty - catch all)
- Actions: Retrieve from backup API

### Graceful Data Handling

Handle missing data without stopping the workflow.

**Actions:**
- Set: Transform customer data

**Catch errors:**
- Conditions: `{{ error.message }}` contains `"missing"`
- Actions: Set default customer values

### Conditional Error Handling

Different responses for different error types.

**Catch rule 1:**
- Conditions: `{{ error.response.info.http_code }}` equals `404`
- Actions: Log "Resource not found" message

**Catch rule 2:**
- Conditions: `{{ error.message }}` contains `"timeout"`
- Actions: Wait 5 seconds, retry API call

**Catch rule 3:**
- Conditions: (empty)
- Actions: Send error notification to admin

### Error Logging

Continue processing but log errors for review.

**Actions:**
- Send: Push data to external system

**Catch errors:**
- Actions: 
  - Trace: Log error details
  - Set: Mark record as "failed"

## Best Practices

**Use specific catch rules first**: Order catch rules from most specific to most general. More specific conditions should come first, with a catch-all rule at the end if needed.

**Keep actions list focused**: Only wrap actions in Attempt that might fail. Don't wrap your entire automation unnecessarily.

**Log caught errors**: Even though the automation continues, log caught errors for debugging and monitoring.

**Test error scenarios**: Use the Trace task to simulate errors and verify your catch rules work correctly.

**Don't catch everything blindly**: Consider which errors should actually stop the automation versus which can be handled gracefully.

**Use descriptive error conditions**: Make it clear what each catch rule is handling for easier maintenance.

## Error Context

When an error is caught, the `{{ error }}` variable provides context about what went wrong. The exact fields available depend on the error source:

**Common fields:**
- `message` - Human-readable error description
- `type` - Error category or classification
- `code` - Error code (if applicable)

**For API/connection errors:**
- `response.info.http_code` - HTTP status code
- `response.body` - Response body content
- `response.headers` - Response headers

## Notes

- Errors are logged as notices when caught, preserving error history without marking the run as failed
- Catch rules are evaluated in order - only the first matching rule executes
- System-level PHP errors may not be catchable by this task
- The automation's data state reverts to before the attempted actions when an error is caught
- You can disable individual catch rules without deleting them
- Empty conditions in a catch rule will match all errors
- The `{{ error }}` variable is only available within catch actions
- Nested Attempt tasks are supported - you can attempt actions within catch handlers
