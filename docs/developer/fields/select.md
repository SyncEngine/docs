# Select Field (`type: select`)

Supports simple select and advanced async/grouped select variants.

## Typical Props

- `choices` (object/array)
- `customizable` (switch between predefined and custom input)
- `group`, `filters`
- `async`, `onAsyncSearch`
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
