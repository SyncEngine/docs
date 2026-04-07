# Webservice Field (`type: webservice`)

Loads and renders webservice-mode fields dynamically from a selected webservice type.

UNSTABLE, INTERNALY USE ONLY

## Typical Props

- `webservice` (class or config object)
- `mode` (required, such as `send`/`retrieve`)

## Example

```php
'response' => [
    'label' => $this->trans('Response data'),
    'type' => 'webservice',
    'webservice' => '{{ connection.config.webservice }}',
    'mode' => 'retrieve',
]
```

## Additional Examples

```php
'request' => [
    'label' => $this->trans('Request data'),
    'type' => 'webservice',
    'webservice' => $config['webservice'],
    'mode' => 'send',
]
```

```php
'response' => [
    'type' => 'webservice',
    'webservice' => $config['webservice'],
    'mode' => 'retrieve',
    'collapsed' => true,
]
```
