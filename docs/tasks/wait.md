# Wait Task

## Overview

The **Wait** task pauses automation execution for a specified amount of time. It's a simple but useful utility for adding delays between operations, implementing rate limiting, or waiting for external processes to complete.

## When to Use

Use the Wait task when you need to:

- Add delays between API calls to respect rate limits
- Wait for external processes to complete
- Space out operations over time
- Implement retry delays
- Add buffer time between resource-intensive operations
- Pause before checking status or polling for results
- Control the pace of automated workflows

## Settings

### Seconds to wait

**Required** | **Type**: Number field | **Taggable**: Yes

The number of seconds to pause execution. Supports decimal values for partial seconds.

**Examples:**
- `1` - Wait 1 second
- `5` - Wait 5 seconds
- `0.5` - Wait half a second (500ms)
- `30` - Wait 30 seconds
- `{{ variables.retry_delay }}` - Use a dynamic delay value

**Note**: You can use dynamic tags to calculate wait times based on workflow data or variables.

## How It Works

1. **Get Wait Duration**: Retrieves the configured seconds value
   - If value is a string, converts to float (handles comma decimals)
   - Supports both dot and comma as decimal separators

2. **Pause Execution**: Calls PHP's `sleep()` function to pause for the specified duration
   - Blocks execution completely during the wait
   - No other tasks execute during this time
   - System resources are minimal during wait

3. **Continue**: After the wait completes, execution continues with the next task
   - Data passes through unchanged
   - No modifications to workflow state

## Use Cases

### API Rate Limiting

**Scenario**: Respect API rate limits by spacing out requests.

**Configuration:**
- **Seconds to wait**: `1`

**Result**: Each API call is spaced 1 second apart, preventing rate limit errors.

### Retry Delay

**Scenario**: Wait before retrying a failed operation.

**Implementation** (with Attempt task):
- Attempt: API call
- Catch error:
  - Wait: 5 seconds
  - Retry API call

### Progressive Backoff

**Scenario**: Implement exponential backoff for retries.

**Configuration:**
- First retry: Wait `2` seconds
- Second retry: Wait `4` seconds
- Third retry: Wait `8` seconds

Use variables or calculations to dynamically adjust wait times.

### Status Polling Delay

**Scenario**: Check job status every 10 seconds until complete.

**Implementation** (with Loop):
- Loop:
  - Retrieve: Check job status
  - Choose: If not complete
    - Wait: 10 seconds
    - Continue loop

### Batch Processing Pace

**Scenario**: Process records in batches with pauses between batches.

**Configuration:**
- Process batch
- **Wait**: `3` seconds
- Process next batch

### External Process Wait

**Scenario**: Wait for an external system to process data before continuing.

**Configuration:**
- **Seconds to wait**: `30`

**Result**: Gives external system time to complete its operations.

## Wait Duration Tips

### Short Waits (< 1 second)

Use decimal values for precise short delays:
- `0.1` - 100 milliseconds
- `0.5` - 500 milliseconds
- `0.25` - 250 milliseconds

**Use for**: Fine-grained rate limiting, quick retries

### Medium Waits (1-30 seconds)

Standard delays for most use cases:
- `1` - 1 second
- `5` - 5 seconds
- `10` - 10 seconds
- `30` - 30 seconds

**Use for**: API rate limits, retry delays, polling intervals

### Long Waits (> 30 seconds)

Extended pauses for slower processes:
- `60` - 1 minute
- `300` - 5 minutes
- `600` - 10 minutes

**Use for**: External process completion, scheduled delays

**Note**: Very long waits may time out depending on server configuration. Consider using scheduled automations for delays over 5-10 minutes.

## Dynamic Wait Times

### Using Variables

```
Variables:
  retry_delay: 5

Wait task:
  Seconds: {{ variables.retry_delay }}
```

### Calculated Delays

```
Wait task:
  Seconds: {{ cache.retry_count * 2 }}
```

Results in progressive backoff: 2s, 4s, 6s, 8s...

### Conditional Delays

Use with Choose task to vary delay based on conditions:
```
Choose:
  Option 1 (High priority): Wait 1 second
  Option 2 (Normal priority): Wait 5 seconds
  Default: Wait 10 seconds
```

## Best Practices

**Use Appropriate Durations**: Don't wait longer than necessary. Excessive waits slow down automations unnecessarily.

**Consider Server Timeouts**: Very long waits (>10 minutes) may cause timeout errors. Use scheduled automations for long delays.

**Implement Progressive Backoff**: For retries, increase wait time with each attempt rather than using fixed delays.

**Respect API Limits**: Check API documentation for rate limits and add appropriate waits between calls.

**Test with Shorter Waits**: During development, use shorter wait times for faster testing, then increase for production.

**Document Why You're Waiting**: Add comments or labels explaining the reason for the delay, especially for longer waits.

**Don't Wait in Loops Without Exit**: Always ensure loops with Wait tasks have proper exit conditions to avoid infinite waits.

**Consider Async Alternatives**: If waiting for external processes, consider using webhooks or scheduled checks instead of long synchronous waits.

## Common Patterns

### Rate Limit Pattern

```
Loop through items:
  - Process item (API call)
  - Wait: 1 second
```

### Retry with Backoff

```
Attempt:
  - API call
Catch:
  - Wait: {{ variables.retry_delay }}
  - Retry
```

### Polling Pattern

```
Loop:
  - Check status
  - Choose:
      If not ready:
        - Wait: 5 seconds
        - Continue
      If ready:
        - Break loop
```

### Batch Pacing

```
Loop through batches:
  - Process batch of 100 items
  - Wait: 10 seconds
  - Next batch
```

## Notes

- Wait pauses the entire automation execution thread
- Data is not modified by the Wait task
- Wait time is blocking - no other tasks execute during the wait
- Decimal seconds are supported (e.g., 0.5 for half second)
- Both dot (.) and comma (,) work as decimal separators
- String values are automatically converted to numbers
- Wait of 0 seconds effectively does nothing (no pause)
- Very long waits may be interrupted by server timeouts
- System resources are minimal during waits (efficient)
- Wait does not guarantee precision for very short durations (<0.1s)
- For delays over 10 minutes, consider using scheduled automations instead
- The Wait task does not account for time already elapsed in previous tasks