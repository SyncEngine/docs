# Params Field (`type: params`)

Parameter editor that can switch between grid mode and code mode with format support.

## Typical Props

- `columns`
- `customizable`
- `formats` (format choices and optional extra format-specific fields)
- `taggable`, `sortable`

## Example

```php
'headers' => [
    'label' => $this->trans('Request header'),
    'type' => 'params',
    'collapsed' => true,
    'taggable' => true,
]
```
