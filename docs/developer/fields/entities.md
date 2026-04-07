# Entities Field (`type: entities`)

Repeatable list of entity references with optional per-item config/actions.

## Typical Props

- `entity`
- `query`, `choices`
- `columns`
- `actions`, `create`
- `itemProps`, `itemCallbacks`, `itemActions`, `itemToolbar`, `itemHeader`
- `sortable`, `max`

## Example

```php
'connections' => [
    'label' => $this->trans('Connections'),
    'type' => 'entities',
    'entity' => 'connection',
    'sortable' => true,
]
```
