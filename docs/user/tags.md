# Tags

**Tags** let you use runtime values anywhere in a configuration: data from the current task, variables, cached values, the incoming request and more.
A tag is written between double braces, for example `{{ data.customer.email }}`, and is replaced just before the task runs.

---

## Syntax

| What | Example | Notes |
|---|---|---|
| Path | `{{ data.customer.email }}` | Dot-separated. Spaces inside the braces are optional. |
| List item | `{{ data.lines.0.sku }}` | Use `.0`, not `[0]`. |
| All items | `{{ data.lines.[].sku }}` | A list with the `sku` of every line, e.g. `["A", "B"]`. |
| Key with a dot | `{{ data."meta.key" }}` | Put the part in double quotes. |
| Filter | `{{ data.lines\|count }}` | One filter per tag value, see [Filters](#filters). |
| Dynamic path | `{{ cache.countries.<{ data.country }> }}` | The `<{ ... }>` part is resolved first and used in the path. |

### Fallbacks

| Operator | Example | Falls back when the value is |
|---|---|---|
| `??` | `{{ data.nickname ?? data.name ?? "Guest" }}` | missing or `null` |
| `?:` | `{{ data.nickname ?: "Guest" }}` | empty: `null`, `""`, `0`, `"0"`, `false` or an empty list |

- Fallback values can be other tags or text in double or single quotes (`"Guest"` or `'Guest'`). Without quotes, the text is read as another tag.
- Operators are evaluated **from left to right**: `{{ data.a ?: data.b ?? "x" }}` means "`a` if not empty, otherwise `b`, and if that is missing, `x`".

!!! tip
    Use `??` when an empty value is a valid value (for example `0` stock).
    Use `?:` when an empty text should also be replaced (for example an empty name).

---

## Value types

- A field that contains **only one tag** keeps the original type: lists stay lists, numbers stay numbers.
- A tag **mixed with text** always results in text: `"Order #{{ data.id }}"`.
    - Lists inside text become the word `Array`. Use a filter instead, for example `{{ data.tags|string(", ") }}`.
- Tags **can't do calculations**: `{{ data.total }} / {{ data.qty }}` results in the text `32.75 / 2`. Use the [Calculate](../tasks/calculate.md) task.

### Tags that can't be resolved

In task configurations, a tag that can't be resolved is **removed**: a single tag becomes empty (`null`), a tag inside text becomes empty text.
No error is shown, so if a preview shows an unexpectedly empty value, check the spelling and whether the value exists at that point.

---

## Available values

| Tag | Contains |
|---|---|
| `data` | The data of the current task. Inside a [Loop](../tasks/loop.md) this is the current item. |
| `variables` | Variables of the automation, Trigger task or routine input. See [Variables & Inputs](variables.md). |
| `cache` | Values stored with the [Cache](../tasks/cache.md) task during this run. |
| `request` | Fields of the incoming request body. |
| `query` | Fields of the incoming request URL query string. |
| `vault` | Secrets stored in the vault, for connection credentials. |
| `timestamp` | The current time as a Unix timestamp. `{{ timestamp.-1 day }}` for a relative time. |
| `storage` | Data from a [Storage](storages.md): `{{ storage.<id>.path }}`. |
| `iteration` | Batch information when an automation runs in batches: `current`, `index`, `limit`, `offset`. |
| `context.loop` | Inside a Loop: `data` (current item), `index` (its key) and `iteration.current` / `iteration.size`. |
| `step` | In flow step input and variables: the output of earlier steps, `{{ step.<step reference>.field }}`. |
| `row` | In [Filter](../tasks/filter.md) conditions and [Index](../tasks/index.md) keys: the row being processed. |

---

## Filters

| Filter | Example | Result |
|---|---|---|
| `count` | `{{ data.lines\|count }}` | Number of items |
| `trim` | `{{ data.code\|trim("-") }}` | Text without leading/trailing characters |
| `string` | `{{ data.tags\|string(", ") }}` | List joined into text |
| `format` | `{{ data.lines\|format("json") }}` | List encoded as JSON, CSV, … |
| `date` | `{{ data.created\|date("Y-m-d", "Europe/Amsterdam") }}` | Formatted date |
| `escape` | `{{ data.name\|escape("url") }}` | Escaped for `url`, `rawurl`, `html` or `sql` |

Unknown filters are ignored and the value is used unchanged.

---

## Common mistakes

| Wrong | Right | Why |
|---|---|---|
| `{{ order_id }}` | `{{ variables.order_id }}` | Every tag starts with where the value comes from |
| `{{ params.id }}` | `{{ request.id }}` | Request body fields use `request` |
| `{{ data.lines[0].sku }}` | `{{ data.lines.0.sku }}` | Brackets are not supported |
| `{{ data.name ?? Guest }}` | `{{ data.name ?? "Guest" }}` | Text must be quoted |
| `{{ now }}` | `{{ timestamp\|date("c") }}` | There is no `now` tag |
| `{{ data.a }} * 2` | [Calculate](../tasks/calculate.md) task | Tags don't calculate |

---

## Next steps

- [Variables & Inputs](variables.md) – How variables are passed and inherited.
- [Tasks](tasks.md) – Where tags are used.
