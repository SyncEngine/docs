# Mapper Field (`type: mapper`)

Maps source keys to target keys. Can run in simple grid mode or configurable mode.

## Typical Props

- `choices` (schema/object source)
- `customizable`
- `sortable`, `taggable`
- `config` (if set, mapper renders nested fields instead of default grid)

## Example

```php
'map' => [
    'type' => 'mapper',
    'taggable' => true,
    'choices' => 'schema',
]
```
