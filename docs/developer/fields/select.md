# Select Field (`type: select`)

Supports simple select and advanced async/grouped select variants.

## Typical Props

- `choices` (object/array)
- `customizable` (switch between predefined and custom input)
- `group` (group options by param)
- `filters` (allow filters by param)
- `async`
- `placeholder`, `required`

## Example

```php
'action' => [
    'label' => $this->trans('Action'),
    'type' => 'select',
    'default' => 'value',
    'choices' => [
        'value' => $this->trans('Replace values'),
        'key' => $this->trans('Replace keys'),
    ],
]
```

## Additional Examples

```php
'separator' => [
    'label' => $this->trans('Separator'),
    'type' => 'select',
    'customizable' => true,
    'choices' => [
        ',' => 'Comma',
        ';' => 'Semicolon',
        '{*nl*}' => [ 'label' => 'New line', 'description' => 'Some description', ],
    ],
]
```

```php
'select' => [
    'label' => $this->trans('Separator'),
    'type' => 'select',
    'customizable' => true,
    'group' => [ 'key' => 'type', 'fallback' => $this->trans('Fallback group name') ]
    'filters' => [ 'key' => 'type' ]
    'choices' => [
        'one' => [ 'label': "One", 'type': 'foo' ],
        'two' => [ 'label': "Two", 'type': 'bar', 'description': 'Description text' ],
        'three' => [ 'label': "Three" ],
    ],
]
```
