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

```php
'retry' => [
    'label' => $this->trans('Retry on failure'),
    'type' => 'boolean',
]
```

## Additional Examples

```php
'active' => [
    'label' => $this->trans('Active'),
    'type' => 'checkbox', // Alias
]
```

```php
'preserve_keys' => [
    'label' => $this->trans('Preserve column keys?'),
    'type' => 'checkbox',
    'conditions' => [
        'action' => ['key']
    ],
]
```

```php
'enabled' => [
    'label' => $this->trans('Enabled'),
    'type' => 'boolean',
    'default' => false,
]
```

```php
'flags' => [
    'label' => $this->trans('Flags'),
    'type' => 'toggle',
    'choices' => [
        // Multiple toggles.
        'trim' => 'Trim',
        'lowercase' => 'Lowercase',
        'strict' => 'Strict'
    ],
    'inline' => true,
]
```
