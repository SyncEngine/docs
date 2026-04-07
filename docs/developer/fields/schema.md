# Schema Field (`type: schema`)

Schema editor built on grid with columns for key/type/required/readonly.

## Typical Props

- `source` (`data` supported)
- `choices`
- `customizable`
- `sortable`, `taggable`

## Example

```php
'schema' => [
    'label' => $this->trans('Schema'),
    'type' => 'schema',
    'sortable' => true,
]
```

## Additional Examples

```php
'schema' => [
    'type' => 'schema',
    'source' => 'data',
    'customizable' => true,
]
```

```php
'fields_schema' => [
    'type' => 'schema',
    'choices' => [ // Limit to predefined schema keys.
        'email' => 'Email', 
        'name' => 'Name'
    ],
    'sortable' => true,
]
```
