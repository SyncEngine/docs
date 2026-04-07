# Radio Field (`type: radio`)

Single-choice selector rendered via check/radio controls.

## Typical Props

- `choices`
- `button` (button style)
- `inline`, `vertical`

## Example

```php
'mode' => [
    'label' => $this->trans('Mode'),
    'type' => 'radio',
    'choices' => [
        'fast' => $this->trans('Fast'),
        'safe' => $this->trans('Safe'),
    ],
]
```
