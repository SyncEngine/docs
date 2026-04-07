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

## Additional Examples

```php
'tasks' => [
    'type' => 'tasks',
    'query' => ['type' => 'modifier'],
    'max' => 25,
]
```

```php
'tasks' => [
    'type' => 'tasks',
    'sortable' => true,
    'default' => [
        ['_class' => 'Set', '_label' => 'Initialize'],
        ['_class' => 'Send', '_label' => 'Push result'],
    ],
]
```
