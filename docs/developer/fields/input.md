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

## Additional Examples

```php
'timeout' => [
    'label' => $this->trans('Request timeout'),
    'type' => 'number',
    'placeholder' => '30',
    'postfix' => 'seconds',
]
```

```php
'value_template' => [
    'label' => $this->trans('Value template'),
    'type' => 'text',
    'placeholder' => '{*value*}',
    'taggable' => true,
]
```
