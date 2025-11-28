# Template Task

## Overview

The **Template** task allows you to create custom data transformations using the Twig templating language. It provides advanced users with the ability to write custom logic that goes beyond standard task capabilities, offering full programmatic control over data manipulation.

## When to Use

Use the Template task when you need to:

- Perform complex data transformations not possible with other tasks
- Apply custom logic or calculations
- Manipulate data structures programmatically
- Use advanced string formatting and manipulation
- Implement custom business logic with loops and conditions
- Transform data using Twig's powerful filters and functions
- Create sophisticated data mappings

**Note**: This is an advanced task. Most data operations can be accomplished with standard tasks like Set, Map, Filter, etc. Use Template only when those tasks don't meet your needs.

## Settings

### Template

**Required** | **Type**: Code editor (Jinja2/Twig) | **Taggable**: Yes

Write your custom Twig template code. The template manipulates the `data` variable, which becomes the task's output.

**Important template rules:**
- Output tags `{{ variable }}` are parsed **before** Twig as SyncEngine tags
- Execute statements `{% function %}` are parsed **by** Twig
- The value of `data` at the end determines the task output
- Must output valid data that can be used by subsequent tasks

**Available variables:**
- `data` - Current workflow data (modify this for output)
- `config` - Task configuration
- `cache` - Cache values
- `variables` - Workflow variables
- `context` - Execution context

**Example:**
```twig
{# Modify the data variable #}
{% set data = data|merge({'processed': true}) %}
```

## How It Works

1. **Parse SyncEngine Tags**: The template is first processed for SyncEngine output tags `{{ }}` to resolve any dynamic values

2. **Execute Twig Template**: The resulting template is passed to the Twig engine with available variables

3. **Extract Output**: After template execution, the `data` variable is serialized to JSON and extracted

4. **Return Data**: The modified `data` becomes the task's output for subsequent steps

**Error handling:**
- If template is empty: Error logged, original data returned
- If Twig execution fails: Error logged with exception details, original data returned
- If JSON serialization fails: Error logged, original data returned

## Use Cases

### Custom Data Transformation

**Scenario**: Apply complex business logic that requires conditionals and calculations.

**Template:**
```twig
{# Calculate discount based on customer tier #}
{% if data.customer.tier == 'premium' %}
  {% set discount = data.order_total * 0.20 %}
{% elseif data.customer.tier == 'gold' %}
  {% set discount = data.order_total * 0.10 %}
{% else %}
  {% set discount = 0 %}
{% endif %}

{% set data = data|merge({
  'discount': discount,
  'final_total': data.order_total - discount
}) %}
```

### Build Complex Object

**Scenario**: Construct a nested data structure from flat data.

**Template:**
```twig
{% set data = {
  'customer': {
    'name': data.first_name ~ ' ' ~ data.last_name,
    'contact': {
      'email': data.email,
      'phone': data.phone
    }
  },
  'metadata': {
    'processed_at': 'now'|date('Y-m-d H:i:s'),
    'version': '2.0'
  }
} %}
```

### Loop and Transform

**Scenario**: Iterate over items and apply transformations.

**Template:**
```twig
{% set transformed = [] %}
{% for item in data.products %}
  {% set transformed = transformed|merge([{
    'id': item.sku,
    'name': item.title|upper,
    'price': item.price * 1.2
  }]) %}
{% endfor %}

{% set data = {'products': transformed} %}
```

### Conditional Field Mapping

**Scenario**: Map fields differently based on data type.

**Template:**
```twig
{% if data.type == 'customer' %}
  {% set data = {
    'entity_id': data.customer_id,
    'entity_name': data.customer_name,
    'entity_type': 'customer'
  } %}
{% else %}
  {% set data = {
    'entity_id': data.order_id,
    'entity_name': data.order_number,
    'entity_type': 'order'
  } %}
{% endif %}
```

### String Manipulation

**Scenario**: Advanced text processing and formatting.

**Template:**
```twig
{% set data = {
  'formatted_name': data.name|title,
  'slug': data.title|lower|replace({' ': '-'}),
  'excerpt': data.description|slice(0, 100) ~ '...',
  'tags': data.tags_string|split(',')
} %}
```

## Twig Features

### Control Structures

```twig
{# Conditionals #}
{% if condition %}
  ...
{% elseif other_condition %}
  ...
{% else %}
  ...
{% endif %}

{# Loops #}
{% for item in items %}
  {{ item.name }}
{% endfor %}

{# Variables #}
{% set myvar = 'value' %}
```

### Filters

```twig
{{ data.name|upper }}              {# Uppercase #}
{{ data.name|lower }}              {# Lowercase #}
{{ data.name|title }}              {# Title Case #}
{{ data.description|slice(0, 50) }} {# Substring #}
{{ data.tags|join(', ') }}         {# Join array #}
{{ data|merge(other) }}            {# Merge objects #}
{{ data.date|date('Y-m-d') }}      {# Format date #}
```

### Functions

```twig
{{ range(1, 10) }}                 {# Number range #}
{{ cycle(['odd', 'even'], index) }} {# Cycle values #}
{{ max(values) }}                  {# Maximum value #}
{{ min(values) }}                  {# Minimum value #}
```

## SyncEngine Tags vs Twig

### SyncEngine Tags (Parsed First)

Output tags `{{ }}` are processed **before** Twig:

```twig
{# This resolves SyncEngine variables first #}
{% set customer_id = {{ data.customer.id }} %}
```

Use for accessing SyncEngine-specific data like previous step results.

### Twig Statements (Parsed by Twig)

Execute statements `{% %}` are processed **by** Twig:

```twig
{# This is pure Twig logic #}
{% for item in data.items %}
  {% set item = item|merge({'processed': true}) %}
{% endfor %}
```

## Available Variables

### data

The current workflow data. Modify this variable to change the task output.

```twig
{% set data = data|merge({'new_field': 'value'}) %}
```

### config

The task's configuration settings.

```twig
{{ config.some_setting }}
```

### cache

Access cached values from earlier in the workflow.

```twig
{{ cache.customer_id }}
```

### variables

Workflow variables.

```twig
{{ variables.api_endpoint }}
```

### context

Execution context object (advanced usage).

## Best Practices

**Use Standard Tasks First**: Only use Template when built-in tasks can't accomplish your goal. Standard tasks are easier to maintain and understand.

**Test Incrementally**: Build complex templates step by step, testing each addition.

**Comment Your Code**: Use Twig comments `{# comment #}` to explain complex logic.

**Handle Edge Cases**: Check for empty values, missing fields, and type mismatches.

**Keep Templates Focused**: If your template is very long, consider breaking it into multiple Template tasks or using other tasks for parts of the logic.

**Validate Output**: Ensure your template always produces valid, usable data.

**Mind the Tag Parsing**: Remember that `{{ }}` is parsed as SyncEngine tags first, `{% %}` is Twig.

## Common Patterns

### Merge New Fields

```twig
{% set data = data|merge({
  'new_field': 'value',
  'calculated': data.price * 1.2
}) %}
```

### Replace Entire Data

```twig
{% set data = {
  'clean_structure': {
    'id': data.old_id,
    'name': data.old_name
  }
} %}
```

### Transform Array Items

```twig
{% set results = [] %}
{% for item in data.items %}
  {% set results = results|merge([item|merge({'processed': true})]) %}
{% endfor %}
{% set data = {'items': results} %}
```

### Conditional Processing

```twig
{% if data.total > 1000 %}
  {% set data = data|merge({'tier': 'premium'}) %}
{% endif %}
```

## Error Handling

The Template task catches and logs errors gracefully:

- **Empty template**: "No template set" error, returns original data
- **Twig syntax errors**: Full exception details logged, returns original data
- **Runtime errors**: Exception caught and logged, returns original data
- **Invalid JSON output**: Serialization error logged, returns original data

## Notes

- The Template task uses the Twig templating engine
- Output tags `{{ }}` are parsed by SyncEngine before Twig processing
- Execute statements `{% %}` are parsed by Twig
- The `data` variable must contain the desired output at template end
- Available variables: `data`, `config`, `cache`, `variables`, `context`
- HTML entities in output are decoded automatically
- Output must be valid JSON-serializable data
- Complex Twig features like macros, includes, and extends are not supported
- The template is compiled each execution (not cached)
- Twig auto-escaping is not enabled
- This is an advanced task - use standard tasks when possible for better maintainability