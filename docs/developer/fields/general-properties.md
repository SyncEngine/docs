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
- `inline`: Use inline/grid layout for grouped/nested fields.
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
    'nested' => [
        'field_1' => [
            // .. Field config, stored as `response.field_1`
        ],
        'field_2' => [
            // .. Field config, stored as `response.field_2"
        ]
    ],
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
    'fieldset' => [
        'repeated_field_1' => [
            // .. Field config, stored as `response.[index].repeated_field_1`
        ],
        'repeated_field_2' => [
            // .. Field config, stored as `response.[index].repeated_field_2"
        ]
    ],
]
```

## Advanced Composition Examples

Nested with local inline fields and conditional children:

```php
'send' => [
    'label' => $this->trans('Send current data'),
    'type' => 'switch',
    'fields' => [
        'transport' => [
            'label' => $this->trans('Transport'),
            'type' => 'select',
            'choices' => [
                'custom' => $this->trans('Manual'),
                'body' => $this->trans('Body'),
                'headers' => $this->trans('Headers'),
                'query' => $this->trans('Query'),
            ],
            'conditions' => ['send' => true], // Show if "send" has a non-empty value.
        ],
    ],
]
```

Complex tabbed repeater item (request/response/actions), based on multistep auth:

```php
'authorization' => [
    'type' => 'repeater',
    'actions' => ['disable', 'delete'],
    'fieldset' => [
        '' => [
            'tabs' => [
                'request' => [
                    'label' => $this->trans('Request'),
                    'nested' => [
                        'url' => ['type' => 'text', 'taggable' => true],
                        'method' => ['type' => 'select', 'choices' => ['GET' => 'GET', 'POST' => 'POST']],
                        'headers' => ['type' => 'params', 'taggable' => true],
                    ],
                ],
                'response' => [
                    'label' => $this->trans('Response'),
                    'nested' => [
                        'format' => ['type' => 'select', 'choices' => ['json' => 'JSON', 'xml' => 'XML']],
                        'tags' => [
                            'type' => 'grid',
                            'sortable' => true,
                            'columns' => ['param' => 'Param', 'tag' => 'Tag', 'expiration' => 'Expiration'],
                        ],
                    ],
                ],
                'actions' => [
                    'label' => $this->trans('Actions'),
                    'nested' => [
                        'success' => ['type' => 'select', 'choices' => ['' => 'Next', 'skip' => 'Skip', 'stop' => 'Stop']],
                        'error' => ['type' => 'select', 'choices' => ['' => 'Previous', 'restart' => 'Restart', 'stop' => 'Stop']],
                    ],
                ],
            ],
        ],
    ],
]
```

Wizard/page-style structure for step-by-step setup:

```php
'setup' => [
    'wizard' => [
        'connection' => [
            'label' => $this->trans('Connection'),
            'nested' => [
                'host' => ['type' => 'text', 'required' => true],
                'authentication' => ['type' => 'authentication'],
            ],
        ],
        'request' => [
            'label' => $this->trans('Request'),
            'nested' => [
                'endpoint' => ['type' => 'text', 'taggable' => true],
                'method' => ['type' => 'select', 'choices' => ['GET' => 'GET', 'POST' => 'POST']],
                'body' => ['type' => 'params', 'formats' => ['choices' => ['json' => 'JSON']]],
            ],
        ],
        'response' => [
            'label' => $this->trans('Response'),
            'nested' => [
                'format' => ['type' => 'select', 'choices' => ['json' => 'JSON', 'xml' => 'XML']],
                'schema' => ['type' => 'schema', 'sortable' => true],
            ],
        ],
    ],
]
```

Nested tag-scope override and conditional visibility:

```php
'response' => [
    'label' => $this->trans('Response mapping'),
    'tags' => [
        'response' => '_input',
        'variables' => '_input',
    ],
    'nested' => [
        'enabled' => ['type' => 'toggle'],
        'map' => [
            'type' => 'mapper',
            'choices' => 'schema',
            'conditions' => ['enabled' => true],
        ],
    ],
]
```
