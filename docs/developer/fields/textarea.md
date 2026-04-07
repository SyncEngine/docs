# Textarea Field (`type: textarea`)

Alias of `input` with multiline enabled.

```php
'note' => [
    'label' => $this->trans('Note'),
    'type' => 'textarea',
    'taggable' => true,
]
```

## Additional Examples

```php
'description' => [
    'label' => $this->trans('Description'),
    'type' => 'textarea',
    'placeholder' => $this->trans('Enter a longer explanation'),
]
```
