# Webservice Field (`type: webservice`)

Loads and renders webservice-mode fields dynamically from a selected webservice type.

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
