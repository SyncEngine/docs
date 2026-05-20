# Architecture Overview

The **SyncEngine architecture** is designed to be modular, scalable, and transparent.  
It combines the flexibility of Symfony with a powerful automation core that can handle everything from small data syncs to complex enterprise integrations.

This page gives you a deep look at how SyncEngine is structured and how its components interact.

---

## Core Principles

SyncEngine is built around five key architectural principles:

1. **Modularity** – Every feature, task, and connection is implemented as a replaceable item.
2. **Transparency** – All actions are traceable through detailed logs and dashboards.
3. **Reusability** – Components like Routines and Tasks can be reused across different Flows.
4. **Extensibility** – Developers can extend SyncEngine easily using Composer-installed modules.
5. **Reliability** – Robust error handling, retries, and queueing ensure consistent execution.

---


## Data Flow Lifecycle

Here’s what happens when an automation runs:

1. **Trigger Activated** – A schedule, event, or manual trigger starts the automation.
2. **Source Resolution** – The automation resolves its configured source and prepares the initial input data.
3. **Action Loading** – The automation loads the configured actions, such as tasks, a routine, or a flow.
4. **Routine / Flow Execution** – Routines and flows run in sequence, passing transformed data forward.
5. **Task Execution** – Tasks process data, call modules or external APIs, and update the execution context.
6. **Result Delivery** – The final dataset is returned and may also be sent to a destination connection.
7. **Logging & Storage** – Every step is logged and stored for traceability.

### Execution Outcomes

Execution success is based on the full execution context, not only on whether individual actions produced data or sent requests.

This means an automation can still perform useful work and end with `success = false` if one or more tasks recorded errors during execution.

When reviewing an execution result, inspect:

- the final returned data
- the execution errors and logs
- any outbound request traces that are relevant to the flow

---

## Message Queue Integration

SyncEngine by default uses **Symfony Messenger** for background processing and asynchronous task execution.  
This allows it to handle large volumes of data without blocking the main thread. But SyncEngine also accepts different que managers, these are easily set for advanced users.

### Features of the Queue System
- Retry strategies with exponential backoff
- Failure transport for error isolation
- Real-time que monitoring via the dashboard

> **Tip:** You can configure your queue transport in interface at "System > Processes" .

### Automation Execution Modes

Run admission is decided when scheduling a run:

- **`single`**: allow one active run, reject new requests while active.
- **`parallel`**: allow concurrent runs.
- **`queued`**: allow one active run, persist incoming requests as queued traces.

Mode switches are not retroactive queue cancellation. Already queued/scheduled runs are still processed; only new incoming requests use the new mode policy.

### Scheduling and Processing Lifecycle

At a high level, a run follows this lifecycle:

1. `schedule()` receives the request and context.
2. A `TraceModel` is created/attached and persisted.
3. Scheduler decides immediate dispatch vs queued trace persistence (mode-dependent).
4. `AutomationBatch` message is dispatched with automation/trace identifiers and request payload.
5. `AutomationBatchHandler` loads the trace, restores request context, and calls `execute()`.
6. `execute()` updates trace status/iteration state and may schedule continuation batches for iterators.
7. When a run finishes, the next iteration batch (see below) or the next queued trace (if any) is dispatched.

Key invariant used by the runtime: background execution requires a valid trace identifier, so run state remains traceable end-to-end.

### Iterator Continuations

When iterator mode is enabled on an automation, one trace can span multiple scheduled batches:

1. First execution starts the trace and advances iteration to batch `1`.
2. If the current result size reaches the configured limit, execution marks the trace as scheduled and queues a continuation.
3. Continuation messages execute the same trace and advance iteration (`2`, `3`, ...).
4. Iteration metadata (index, offset, limit, current) is derived from trace state, not global automation state.
5. When a batch no longer reaches the iterator limit, iterator state is finalized and the trace completes.

This keeps each multi-batch run consistent and auditable as a single trace timeline.

---

## Extending SyncEngine

Developers can extend SyncEngine in three primary ways:

| Extension Type | Description |
|----------------|--------------|
| **Modules** | Add new tasks, connections, or triggers via Composer packages. |
| **Event Listeners** | Hook into internal lifecycle events (before/after flow execution, etc.). |
| **Custom Services** | Inject your own services for specialized logic or integrations. |

---

## Technology Stack

| Layer | Technology                                      |
|-------|-------------------------------------------------|
| Framework | [Symfony 7+](https://symfony.com/)              |
| Backend Language | PHP 8.3+                                        |
| Frontend | Vue.js (with Tailwind CSS)                      |
| Database | SQLite, MySQL, or PostgreSQL                    |
| Caching | Redis or filesystem                             |
| Queueing | Symfony Messenger (Doctrine or Redis transport) |
| Templates | Twig                                            |
| Configuration | YAML / ENV variables                            |

---
## Related Topics

- [Modules](modules.md) – Learn how to extend SyncEngine functionality.
- [API](api.md) – Interact programmatically with SyncEngine.
- [Tasks](../user/tasks.md) – Understand how SyncEngine executes actions.

---

> **Note:** SyncEngine’s architecture is designed to evolve. Each component, from flows to modules, can be replaced or extended without breaking the system, ensuring long-term scalability and maintainability.
