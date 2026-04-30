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
