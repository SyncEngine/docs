# Sequence Field (`type: sequence`)

Specialized entity sequence editor with optional sidebar/modal behavior.

UNSTABLE, INTERNALY USE ONLY

## Typical Props

- inherits `entities`-style behavior
- `entity` (default step entity is `routine`)
- `preview`

## Example

```php
'sequence' => [
    'type' => 'sequence',
    'entity' => 'routine',
]
```

## Additional Examples

```php
'steps' => [
    'type' => 'sequence',
    'entity' => 'routine',
    'preview' => true,
]
```

```php
'steps' => [
    'type' => 'sequence',
    'query' => ['where' => ['enabled' => true]],
]
```
