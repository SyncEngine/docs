# Dataset Field (`type: dataset`)

Data editor that can show as JSON code or grid-like controls depending on `type`.

UNSTABLE, INTERNALY USE ONLY

## Typical Props

- `type` (`mapper`, `fields`, or generic dataset)
- `storageConfig`
- `columns`
- `taggable`

## Example

```php
'dataset' => [
    'type' => 'dataset',
    'columns' => [
        'key' => $this->trans('Key'),
        'value' => $this->trans('Value'),
    ],
]
```
