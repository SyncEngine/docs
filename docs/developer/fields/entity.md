# Entity Field (`type: entity`)

Selects one entity and optionally renders config fields for that entity.

## Typical Props

- `entity` (required entity type)
- `query`, `choices`
- `actions` (`edit`, `create`, `config`)
- `config` (string/object config form definition)

## Example

```php
'connection' => [
    'label' => $this->trans('Connection'),
    'type' => 'entity',
    'entity' => 'connection',
    'actions' => ['edit', 'create'],
]
```
