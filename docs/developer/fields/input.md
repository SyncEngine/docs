# Input Field (`type: text` and fallback)

Default text-like field used by task configs (`text`, `number`, password-like types via `attr/type`).

## Typical Props

- `label`, `description`, `help`
- `placeholder`, `required`
- `taggable`
- `prefix`, `postfix`
- `multiline`/`textarea` behavior (`textarea` alias sets multiline)

## Example

```php
'key' => [
    'label' => $this->trans('Key / Column name'),
    'type' => 'text',
    'taggable' => true,
    'help' => $this->trans('Nested keys are supported'),
]
```
