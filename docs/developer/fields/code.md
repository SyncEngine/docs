# Code Field (`type: code`)

CodeMirror editor with optional language extension and tag insertion.

## Typical Props

- `language`
- `basicSetup` // CodeMirror options.
- `taggable`
- `contained`

## Example

```php
'template' => [
    'label' => $this->trans('Template'),
    'type' => 'code',
    'taggable' => true,
]
```

## Additional Examples

```php
'script' => [
    'label' => $this->trans('Transform script'),
    'type' => 'code',
    'language' => 'javascript',
    'basicSetup' => ['lineNumbers' => true],
]
```

```php
'payload' => [
    'type' => 'code',
    'contained' => true,
    'taggable' => true,
]
```
