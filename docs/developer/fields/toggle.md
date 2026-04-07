# Toggle Field (`type: toggle`)

Boolean/switch style field. Aliases: `boolean`, `checkbox`, `switch`.

## Typical Props

- `default` boolean
- `button`, `variant`
- `choices` for multi-toggle mode
- `inline`, `vertical`

## Example

```php
'recursive' => [
    'label' => $this->trans('Replace recursively?'),
    'type' => 'switch',
]
```
