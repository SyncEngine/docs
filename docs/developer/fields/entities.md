# Entities Field (`type: entities`)

Repeatable list of entity references with optional per-item config/actions.

## Typical Props

- `entity`
- `query`, `choices`
- `columns`
- `actions`, `create`
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

## Additional Examples

```php
'related' => [
    'type' => 'entities',
    'entity' => 'storage',
    'actions' => ['create', 'edit', 'delete'],
    'columns' => ['info' => ['badge' => 'Storage #{{id}}']],
]
```

```php
'connections' => [
    'type' => 'entities',
    'entity' => 'connection',
    'query' => ['where' => ['enabled' => true]],
    'max' => 3,
]
```
