# Authentication Field (`type: authentication`)

Webservice-auth configuration field. Selects a webservice auth type and renders auth/connect sub-fields.

## Typical Props

- `webserviceTypes`
- `query`
- value shape usually includes `_class` and `_connect`

## Example

```php
'authentication' => [
    'label' => $this->trans('Authentication'),
    'type' => 'authentication',
]
```

## Additional Examples

```php
'authentication' => [
    'label' => $this->trans('Authentication'),
    'type' => 'authentication',
    'query' => ['enabled' => true],
]
```

```php
'authentication' => [
    'type' => 'authentication',
    'webserviceTypes' => $types,
    'help' => $this->trans('Configure auth and optional connect handshake'),
]
```
