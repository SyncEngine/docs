# Routines

**Routines** are the backbone of every Flow in SyncEngine.  
They define *how* a specific set of actions (Tasks) should be executed to accomplish a logical part of a workflow.

Each routine is reusable, modular, and can be called by one or more Flows, giving you incredible flexibility and efficiency.

---

## What Is a Routine?

A **Routine** is a sequence collection of **Tasks** that perform a specific function.  
For example, you might have a routine that retrieves orders from a database, another that filters the results, and another that sends them to an external API.

Routines make complex automations easier to manage and maintain by grouping related actions into logical units.  

---

## Routine Structure

```
Routine: Process Active Users
  ↓
  Task: Retrieve (Database)
  Task: Filter (status = active)
  Task: Map (normalize fields)
  Task: Send (to CRM API)
```

This routine retrieves all users, filters active ones, transforms the data, and sends it to another system.
Each task runs in the order you define.  
The output of one task becomes the input of the next, enabling a clean, predictable data flow.
The example that you see above could be run on a daily basis with automatic schedule to keep your CRM updated with your external webshop.
---

## Reusable Logic

Routines can be reused in multiple flows and automations.  
When you update a routine, all flows using it will benefit from those improvements automatically.

Example use cases for reusable routines:

| Routine Name | Description |
|---------------|--------------|
| **Retrieve Orders** | Fetches all active orders from your e-commerce database. |
| **Process Customers** | Cleans and standardizes customer information. |
| **Send to CRM** | Sends processed data to your CRM system. |
| **Error Handler** | Logs and stores failed records for later inspection. |

---

## Routine Configuration Options

| Setting         | Description                                                                     |
|-----------------|---------------------------------------------------------------------------------|
| **Name**        | The routine’s identifier used across flows.                                     |
| **Description** | Optional, describes what the routine does.                                     |
| **Conditions**  | Optional, set required parameters to triggering this routine                   |
| **Schema**      | Optional, set input variables and output schema for a more understandable flow |
---

## Routine Composition Tips

- **Keep routines focused:** A routine should perform one logical function.
- **Use descriptive names:** This makes debugging and collaboration easier.
- **Leverage variables and templates:** Use Twig syntax like `{{ data.email }}` to dynamically modify your data.
- **Centralize reusable logic:** Routines like “Retrieve Data,” “Format Data,” and “Send Data” can be used across multiple flows.

---

## Routine and Flow Relationship

While **Flows** define the *sequence* of execution, **Routines** define the *content* of each step.  
In other words:

| Concept | Purpose |
|----------|----------|
| **Flow** | Defines when and how routines are executed. |
| **Routine** | Defines what happens inside each step (specific logic). |

---

## Best Practices

- Build reusable routines for common actions.
- Keep data transformation tasks together.
- Use `Trace` tasks to debug or log intermediate results.
- Test each routine independently using the **Preview Function** before embedding it in larger flows.

---

## Next Steps

- [Flows](flows.md) – Learn how routines are organized inside flows.
- [Tasks](tasks.md) – Explore the types of actions you can add to a routine.
- [Storages](storages.md) – Store persistent data that routines can access later.

---

> **Tip:** Think of routines as “mini automations.” The more modular and reusable they are, the more scalable your SyncEngine setup becomes.
