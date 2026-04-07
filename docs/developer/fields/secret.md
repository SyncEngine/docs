# Secret Field (`type: secret`)

Secret selector/input hybrid. Supports selecting vault secrets or custom direct values.

## Typical Props

- `customizable` (toggle between secret and custom input)
- `help`, `label`, `description`
- works with `{{ vault.NAME }}` style values

## Example

```php
'api_key' => [
    'label' => $this->trans('API key'),
    'type' => 'secret',
    'customizable' => true,
]
```
