# Tasks Reference

**Tasks** are the building blocks of every SyncEngine automation.  
Each task performs a specific action, such as filtering data, looping over records, sending data to a connection, or triggering another flow.

SyncEngine includes a comprehensive set of **core tasks**, and you can extend this list infinitely through external or custom modules.

---

## What Are Tasks?

In SyncEngine, tasks are executed inside **[Routines](routines.md)** and **[Flows](flows.md)**.  
Each task receives data, processes it, and passes the result to the next task in line.  
This design makes automations flexible, modular, and easy to debug.

---

## Core Tasks List

Below is a list of the core tasks available in SyncEngine by default.

| Task                                 | Description |
|--------------------------------------|--------------|
| **[Attempt](../tasks/attempt.md)**   | Execute actions while gracefully handling any errors that may occur. Think of it as a "try-catch" block for your automation workflows. |
| **[Cache](../tasks/cache.md)**       | Get or set a value in the context cache. This is useful for storing temporary results between tasks or reusing data in different parts of a flow. |
| **[Choose](../tasks/choose.md)**     | Branch logic that selects different execution paths based on conditions. Similar to "if/else" logic. |
| **[Extract](../tasks/extract.md)**   | Extract a single column or property from each item in your dataset. Great for flattening nested structures. |
| **[Filter](../tasks/filter.md)**     | Filter incoming data based on one or more conditions. Only records that match the criteria continue to the next task. |
| **[Group](../tasks/group.md)**       | Group data into sets based on shared properties or values. Perfect for aggregations or categorization tasks. |
| **[Index](../tasks/index.md)**       | Reindex or reorganize your data with a custom index key. Useful for mapping datasets or simplifying lookups. |
| **[Loop](../tasks/loop.md)**         | Iterate over a list of items and execute the following tasks for each one. |
| **[Map](../tasks/map.md)**           | Map key-value pairs, transform data structures, or rename fields dynamically. |
| **[Merge](../tasks/merge.md)**       | Combine multiple datasets or columns into a single unified structure. |
| **[Replace](../tasks/replace.md)**   | Search for and replace text or values within your data. |
| **[Retrieve](../tasks/retrieve.md)** | Retrieve data from a specific connection (e.g., API, database, or storage). Often used as the first task in a flow. |
| **[Send](../tasks/send.md)** | Send processed data to a connection. For example, posting to an API or writing to a database. |
| **[Set](../tasks/set.md)** | Define or modify custom values in your current dataset or execution context. |
| **[Sort](../tasks/sort.md)** | Sort your data ascending or descending by one or more keys. |
| **[Split](../tasks/split.md)** | Split strings or dataset columns into multiple parts or rows. |
| **[Store](../tasks/store.md)** | Get or set a value in a storage container. Storages are long-term containers that persist between runs. |
| **[Template](../tasks/template.md)** | Apply a [Twig](https://twig.symfony.com/) template to dynamically modify data, create content, or format messages. |
| **[Trace](../tasks/trace.md)** | Log the current status, data, or error message. Useful for debugging or tracking data states during execution. |
| **[Trigger](../tasks/trigger.md)** | Trigger another flow independently from the current one. Enables chained or parallel automation. |
| **[Wait](../tasks/wait.md)** | Pause execution for a defined amount of time before continuing. Ideal for throttling or delayed actions. |

---

## Working with Tasks

Every **Task** in SyncEngine is a self-contained action, a specific step your automation performs, configured precisely to your needs.

Each task comes with its own set of **configuration fields**, accessible directly in SyncEngine's interface without any coding experience!  
These fields let you define exactly *what* the task does, *where* it sends or retrieves data from, and *how* it transforms information along the way.

Think of tasks as the **workfoce** of your automation, each performing a specific function that, together, create powerful workflows.

SyncEngine’s **core task library** already provides an extensive foundation, a versatile toolkit capable of handling most automation scenarios right out of the box.  
With a bit of understanding of the systems you’re connecting, you can achieve **virtually anything** using just the built-in tasks.  
And when you want to go even further, **modules** expand your toolkit with specialized or simplified tasks, making complex integrations effortless.

## Advanced Concepts

### Context Cache
The cache task allows you to temporarily store data for each run or reuse data across tasks.  
Think of it as a short-term memory for your automation logic.

### Storage vs Cache
Storages persist across multiple runs and reboots, while cache only exists for the lifetime of a single flow execution.

### Dynamic Variables
Tasks can use dynamic variables such as `{{ data.field_name }}` or `{{ context.cache.my_value }}` inside any text or field.  
This is powered by the Twig templating engine and allows incredible flexibility.

---

## Extending the Task System

SyncEngine is designed to be **modular and extensible**.  
You can add new task types by installing or creating your own modules. For more information about developing your own [module](../developer/modules.md), task or anything you would possibly need read the developer docs.

## Next Steps

- [Flows](flows.md) – Learn how tasks fit together to create complete workflows.
- [Routines](routines.md) – Group and reuse sequences of tasks.
- [Storages](storages.md) – Persist custom data between flows.

---

> **Tip:** Tasks are the DNA of SyncEngine automations. The more you understand how they work and interact, the more powerful your automations become.
