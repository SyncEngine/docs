# Tasks Field (`type: tasks`)

Repeatable task-sequence editor. Each row uses the selected task type's `fields` definition.

## Typical Props

- `taskTypes`
- `query`
- `sortable`, `max`
- includes built-in toolbar add, copy/paste, preview, disable, delete

## Example

```php
'flow' => [
    'label' => $this->trans('Tasks'),
    'type' => 'tasks',
    'sortable' => true,
]
```
