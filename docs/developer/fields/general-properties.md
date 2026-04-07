# General Field Properties And Structural Props

This page describes shared field keys used across task/webservice `getFields()` configs and how the form renderer composes nested UIs.

## Shared Properties

Common keys seen across task and webservice field definitions:

- `type`: Field type (`text`, `select`, `grid`, `entity`, etc.).
- `label`: Visible label.
- `description`: Extra text under/near the field.
- `help`: Tooltip/help content (string or list).
- `default`: Default value when no value is set.
- `required`: Required marker and validation hint.
- `placeholder`: Placeholder for text-like inputs.
- `disabled`, `readonly`: Disable editing.
- `taggable`: Enables tag insertion support on compatible inputs.
- `choices`: Select/radio/toggle/grid option source.
- `customizable`: Allow custom values in certain fields (for example `select`, `mapper`, `schema`).
- `sortable`: Enables drag-sort in repeatable/grid-like fields.
- `actions`: Item-level actions for `entities`, `repeater`, `tasks`, etc.
- `query`: Server/entity filter query for entity/webservice/lookup fields.
- `config`: Additional config payload for specialized fields.
- `columns`: Column definitions for `grid`-based fields.
- `conditions`: Conditional visibility rules.
- `collapsed`: Initial collapsed state when wrapped in a container.
- `icon`: Header/icon hint for wrapped containers.

## Structural Props

The renderer supports composition patterns beyond a single `type`:

- `tabs`: Render child configs in tab panes.
- `wizard` or `pages`: Render child configs as paged navigation.
- `fields`: Inline child fields group (same level context).
- `nested`: Nested field object with its own `Fields` state object.
- `inline`: Layout hint for grouped/nested fields.
- `wrap`: Force wrapping in a `FieldContainer` card.
- `tags`: Scoped tag context overrides for nested structures.

## Conditional Rendering

`conditions` are evaluated by `useConditions`.

Typical forms:

- equality: `"conditions" => ["action" => "send"]`
- list inclusion: `"conditions" => ["action" => ["key", "both"]]`
- operator object: `"conditions" => ["map_source" => ["operator" => "empty"]]`

## Minimal Examples

Basic field:

```php
'key' => [
    'label' => $this->trans('Key'),
    'type' => 'text',
    'taggable' => true,
]
```

Field with nested config:

```php
'response' => [
    'label' => $this->trans('Response data'),
    'nested' => $this->getResponseFields(),
]
```

Tabbed construction:

```php
'' => [
    'tabs' => [
        'request' => ['label' => $this->trans('Request'), 'nested' => $this->getRequestFields()],
        'response' => ['label' => $this->trans('Response'), 'nested' => $this->getResponseFields()],
    ],
]
```

Repeater with actions:

```php
'authorization' => [
    'type' => 'repeater',
    'actions' => [
        'run' => ['type' => 'request', 'props' => ['action' => 'authorize']],
        'disable',
        'delete',
    ],
    'fieldset' => $this->getAuthStepFields(),
]
```
