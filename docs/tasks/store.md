# Store Task

## Overview

The **Store** task allows you to save and retrieve persistent data using SyncEngine's storage system. Unlike the Cache task which stores temporary data for a single automation run, the Store task works with persistent storage that survives across multiple runs and remains available until explicitly cleared.

## When to Use

Use the Store task when you need to:

- Save data that persists across multiple automation runs
- Track state or progress over time (counters, last sync dates, etc.)
- Share data between different automations
- Build up historical data or maintain running totals
- Store API tokens or configuration data for reuse
- Remember the last processed ID or record
- Maintain a persistent queue or list across runs

## Settings

### Key / Column name

**Type**: Text field | **Taggable**: Yes

Specifies which part of your current data to store or where to place retrieved data.

- Use a simple column name (e.g., `customer_id`)
- Access nested data using dot notation (e.g., `order.total`)
- Leave **empty** to work with all data at root level

**For Set action**: The data at this key will be stored
**For Get action**: Retrieved data will be placed at this key

**Examples:**
- `last_sync_date` - Store/retrieve sync date
- `customer.processed_count` - Nested counter
- (empty) - Store/retrieve entire data

### Action

**Required** | **Type**: Select dropdown | **Default**: Set storage

Choose the operation to perform:

- **Set storage**: Save data to persistent storage
- **Get storage**: Retrieve data from persistent storage

### Storage

**Required** | **Type**: Entity selector

Select which storage to use. Storages are configured separately in SyncEngine and can be:

- **JSON storage**: Structured data storage (arrays, objects)
- **Raw storage**: Simple scalar value storage (text, numbers)

You can edit existing storages or create new ones directly from this field.

### Storage column key/path

**Type**: Text field | **Taggable**: Yes

The path within the storage where data will be saved or retrieved. Use dot notation to nest data within the storage structure.

- Leave **empty** to work with the entire storage
- Use dots to create nested structures (e.g., `customers.latest`)
- For JSON storage only (raw storage doesn't support paths)

**Examples:**
- (empty) - Store/retrieve entire storage content
- `last_id` - Store at the "last_id" key
- `customers.count` - Nested counter at customers.count

### Method

**Type**: Select dropdown | **Default**: Replace | **Conditional**: Only when Action is "Set storage"

Choose how to handle the data if something already exists at the storage path:

- **Replace**: Completely replace existing data with new data
- **Merge**: Combine new data with existing (for objects/arrays)
- **Append**: Add new data to existing as an array item

**When to use each:**
- **Replace**: Update a counter, store latest value, refresh data
- **Merge**: Add new properties to objects, combine arrays
- **Append**: Build lists, accumulate items over time

### Not found action

**Type**: Select dropdown | **Default**: Skip | **Conditional**: Only when Action is "Get storage"

What should happen if the storage path doesn't contain data:

- **Override with empty value**: Continue with an empty value
- **Skip task**: Don't execute this task at all

## How It Works

### Set Storage

1. **Retrieve Storage**: Loads the specified storage configuration

2. **Get Source Data**: Extracts data from the **Key / Column name** in current workflow data (or uses all data if key is empty)

3. **Validate Storage Type**:
   - For non-iterable data (scalars): Enforces or converts to raw storage
   - For iterable data (arrays/objects): Requires JSON storage (not raw)

4. **Apply Method**:
   - **Replace**: Overwrites data at the storage path
   - **Merge**: Combines new data with existing data
   - **Append**: Adds new data to existing as array item

5. **Persist**: Saves the storage permanently

### Get Storage

1. **Retrieve Storage**: Loads the specified storage configuration

2. **Get Stored Data**: Retrieves data from the **Storage column key/path** (or entire storage if path is empty)

3. **Handle Not Found**:
   - If data exists or **Not found action** is "Override": Continues
   - If data doesn't exist and **Not found action** is "Skip": Task doesn't execute

4. **Merge into Workflow**:
   - If **Key** is specified: Places data at that key in current workflow data
   - If **Key** is empty: Replaces entire workflow data with stored data

## Use Cases

### Track Last Processed ID

**Scenario**: Remember which record was processed last to avoid duplicates on next run.

**Set Storage** (at end of automation):
- **Key**: `last_id`
- **Action**: Set storage
- **Storage**: `sync_state`
- **Storage path**: `last_processed_id`
- **Method**: Replace

**Get Storage** (at start of next run):
- **Action**: Get storage
- **Storage**: `sync_state`
- **Storage path**: `last_processed_id`
- **Key**: `start_from_id`

### Maintain Running Total

**Scenario**: Keep a cumulative count of processed items across automation runs.

**Configuration:**
- **Key**: `processed_count`
- **Action**: Set storage
- **Storage**: `counters`
- **Storage path**: `total_processed`
- **Method**: Append

Result: Each run adds to the total count stored persistently.

### Store API Configuration

**Scenario**: Save API credentials or endpoints for reuse across automations.

**Configuration:**
- **Key**: (empty - store entire config object)
- **Action**: Set storage
- **Storage**: `api_config`
- **Method**: Replace

### Build Historical Data

**Scenario**: Accumulate daily reports over time.

**Configuration:**
- **Key**: `today_report`
- **Action**: Set storage
- **Storage**: `daily_reports`
- **Storage path**: `{{ variables.today_date }}`
- **Method**: Replace

### Retrieve Stored Configuration

**Scenario**: Load previously saved settings at the start of an automation.

**Configuration:**
- **Action**: Get storage
- **Storage**: `app_settings`
- **Key**: `config`

Result: Stored settings are loaded into the `config` key for use in the automation.

### Merge Customer Data

**Scenario**: Add new customer properties without losing existing ones.

**Configuration:**
- **Key**: `new_customer_data`
- **Action**: Set storage
- **Storage**: `customers`
- **Storage path**: `{{ data.customer_id }}`
- **Method**: Merge

## Storage Types

### JSON Storage

Stores structured data (arrays, objects). Supports nested paths with dot notation.

**Best for:**
- Complex data structures
- Nested objects
- Multiple related values
- Organizing data hierarchically

**Example structure:**
```json
{
  "customers": {
    "count": 150,
    "last_id": 1234
  },
  "sync": {
    "last_run": "2025-11-28"
  }
}
```

### Raw Storage

Stores simple scalar values (text, numbers, booleans). Does not support paths.

**Best for:**
- Single values
- Counters
- Flags
- Simple state

**Note**: If you try to store non-scalar data without a path, the storage type is automatically enforced as raw.

## Best Practices

**Choose the Right Storage Type**: Use JSON storage for structured data, raw storage for simple values.

**Use Descriptive Storage Names**: Name storages clearly - `customer_sync_state`, `api_tokens`, `daily_counters`.

**Leverage Paths for Organization**: Use dot notation to organize related data within a single storage:
- `counters.customers`
- `counters.orders`
- `state.last_sync`

**Remember Storage is Persistent**: Unlike cache, storage data remains until explicitly cleared or replaced. Plan your data lifecycle accordingly.

**Handle Missing Data**: Always configure the **Not found action** when getting storage to handle cases where data doesn't exist yet.

**Use Method Strategically**:
- Replace for updates
- Merge for additive changes
- Append for accumulation

**Consider Storage Cleanup**: Implement periodic cleanup or archiving for storage that grows over time.

**Test Storage Paths**: Verify your dot notation paths work correctly, especially with dynamic tags.

## Differences from Cache

| Feature | Cache | Store |
|---------|-------|-------|
| **Persistence** | Current run only | Survives across runs |
| **Scope** | Single automation run | All automations can access |
| **Use Case** | Temporary workspace | Long-term state |
| **Access** | `{{ cache.key }}` | Via Store task only |
| **Cleanup** | Automatic after run | Manual or never |

## Notes

- Storage persists indefinitely until explicitly cleared or replaced
- Raw storage cannot use paths - path configuration triggers an error
- Attempting to store non-iterable data without a path automatically converts storage to raw type
- Storage is shared across all automations - changes in one automation affect others
- JSON storage supports complex nested structures with dot notation
- The storage entity must be configured separately before use
- Storage can be created or edited directly from the task configuration
- Dynamic tags work in all storage paths and keys
- Empty values can be stored (null, empty arrays, empty strings)
- Storage changes persist immediately when the task executes
- Multiple automations can read/write the same storage simultaneously