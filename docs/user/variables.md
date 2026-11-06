# Variables & Inputs

Besides the data that flows from task to task, SyncEngine passes **variables** down through your automations, flows and routines.
This page explains where variables come from, how they are inherited, and when a value you pass to a routine or flow step actually overrides an existing one.

---

## Where variables come from

| Source | Example |
|---|---|
| Automation **Variables** | `order_id` = `{{ request.order_id }}` |
| [Trigger](../tasks/trigger.md) task **Variables** | `mode` = `test` |
| Routine **input variables** (set when calling a routine from a Trigger) | `customer_id` = `{{ data.customer.id }}` |
| Flow step **input variables** | `order_number` = `{{ step.get_order.number }}` |

Use them anywhere in a configuration with `{{ variables.name }}`.

---

## Inheritance

Variables flow **downwards**:

- A triggered routine, flow or automation sees all variables of its parent.
- Values you pass to it are added on top, and override parent values with the same name.
- Changes never flow back up to the parent.

To share values *upwards* or between sibling steps, use the [Cache](../tasks/cache.md) task. To keep values between runs, use a [Storage](storages.md).

---

## Task variables vs. input variables

There are two kinds of variables you can pass, and they behave slightly differently.

### Task variables

The **Variables** field of the [Trigger](../tasks/trigger.md) task is a free list of names and values.
Every name you add overrides the inherited variable with the same name, **whatever its value**, even an empty one.
If you don't want to override a variable, don't add it to the list.

### Input variables and input

When a routine defines **input variables** or an **input schema** (see [Routines](routines.md)), calling it from a Trigger or a flow step shows a form with those fields.
These forms always store every field, also the ones you never touched.
Therefore only **configured** fields are passed on. Untouched fields are ignored, so the inherited variable stays available.

A field is **configured** when:

| What you entered | Configured? | Result in the routine |
|---|---|---|
| A value: `abc`, `1`, `0`, `false` | Yes | That value |
| A tag: `{{ data.customer.id }}` | **Yes, always** | The resolved value. **If the tag resolves to nothing, the variable becomes empty (`null`)** |
| Nothing (empty field) | No | The inherited variable (if any) |
| A group of fields that are all empty | No | The inherited variable (if any) |
| A group where at least one field has a value or tag | Yes | The group with its values |

Whether a field is configured is decided by **what you entered**, not by the outcome.
Once you put a tag in a field, you meant to set it. If the tag can't be resolved, you get an empty value
instead of silently falling back to an unrelated variable with the same name.

!!! tip
    - To keep the inherited value, leave the field empty. Don't use a tag that refers to it.
    - `0` and `false` are real values and are passed on.
    - A typo in a tag (`{{ variables.order-id }}` instead of `order_id`) still counts as configured and results in an empty value.

The same rule decides whether a Trigger's **routine input** is used at all.
If none of the input fields are configured, the Trigger falls back to its **Pass current data** setting.

Flow step **input** replaces the data passed to the routine as soon as any of its fields has a value.

Input fields you left empty are still passed to the routine, but as **empty (`null`)** values, both for flow steps
and for Trigger routines. Inside the routine, a fallback like `{{ data.name ?? "Unknown" }}` therefore works for
fields that were not filled in. A tag that resolves to an empty text (`""`) keeps that value; use
`{{ data.name ?: "Unknown" }}` to fall back on any empty value (`""`, `null`, `0`, `false`).
See [Tags](tags.md) for all fallback options.

---

## Next steps

- [Trigger task](../tasks/trigger.md) – Pass variables and data to other routines, flows and automations.
- [Routines](routines.md) – Define input variables and an input schema.
- [Flows](flows.md) – Pass variables and input to each step.
- [Tags](tags.md) – Use variables and data in your configuration.
