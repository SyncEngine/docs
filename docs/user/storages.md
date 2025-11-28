# Storages

**Storages** in SyncEngine are secure, persistent containers used to hold structured data that can be accessed across your automations.  
They act as your **long-term memory**, allowing flows and routines to save and retrieve information between runs.

Storages are ideal for saving reusable or shared data, configuration values, and results that you want to access later, without relying on external databases or APIs.

---

## What Is a Storage?

A **Storage** is a named container that can store any kind of structured data, including text, numbers, JSON, or arrays.  
Storages can be accessed and modified using the **[Store Task](../tasks/store.md)** or directly from within the SyncEngine interface.

Unlike the **Cache**, which is temporary and cleared after a run, storages persist permanently until you manually delete or update them.

---

## When to Use Storages

Storages are useful when you need to:

- Save configuration values or user preferences.
- Keep track of last-run timestamps or cursors.
- Store intermediate results from automations.
- Maintain reference data used by multiple flows.
- Temporarily hold large datasets that can be reused later.

> **Note:** Storages are **not** meant for credentials, logs, or caching!  
> SyncEngine already handles those separately in a secure way.

---

## Working with Storages

When you create a new **Storage**, one of the first steps is to define its **Data Type**, the kind of information this storage will hold and how SyncEngine will interpret it.

You can choose from several data types: **Entities**, **Schema**, **Raw**, **Mapper**, or **Generic**.  
Each type defines a distinct way SyncEngine structures, validates, and interacts with your stored data.

Selecting the correct data type is crucial.  
A **Schema** storage, for instance, behaves very differently from a **Generic** one, influencing how data is read, indexed, and referenced within your automations.

By choosing the right type, you ensure your storage performs optimally and integrates seamlessly with your workflows, keeping your automations clean, organized, and predictable.

> **Tip:** Take a moment to plan your storage structure before creating it, the correct data type choice will save time and avoid mismatched data behavior later on.



## Storage vs Cache

| Feature | Storage | Cache |
|----------|----------|-------|
| **Persistence** | Permanent (until deleted manually) | Temporary (per run) |
| **Use Case** | Store reusable or reference data | Hold transient data during automation execution |
| **Access Scope** | All flows and routines | Current flow only |
| **Storage Medium** | Database-backed | In-memory |

---

## Best Practices

- Use storages for small to medium-sized structured data (not big binary files).
- Organize your storages by function (e.g., `config`, `metadata`, `last_sync`).
- Avoid storing sensitive credentials, use the system’s secure credential manager instead.
- Periodically clean up unused storages to optimize performance.

---

## Example Use Case

**Goal:** Save the ID of the last processed order to avoid duplicates.

```
Flow: Order Sync
  ↓
  Routine: Retrieve New Orders
    ├── Task: Retrieve (API)
    ├── Task: Filter (created_after = {{ storage.last_sync.timestamp }})
  Routine: Process Orders
    └── Task: Send (ERP API)
  Routine: Update Storage
    └── Task: Store (set last_sync.timestamp = {{ timestamp }})
```

This ensures SyncEngine only processes new orders after the last successful sync.

---

## Next Steps

- [Tasks](tasks.md) – Learn how the `Store` task interacts with storages.
- [Flows](flows.md) – Combine storages with logic for efficient workflows.
- [Dashboard](dashboard.md) – Monitor storage updates in your automation logs.

---

> **Tip:** Storages turn SyncEngine into a self-contained data hub, capable of remembering context between automations and making decisions based on past executions.
