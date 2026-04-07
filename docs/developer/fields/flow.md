# Flow Field (`type: flow`)

Visual flow/graph editor based on React Flow (`@xyflow/react`).

## Typical Props

- `entity`
- `spacing`
- `preview`

## Value Shape

Flow values are node-like objects, typically carrying `_ref`, optional `target`, and task data.

## Example

```php
'flow' => [
    'label' => $this->trans('Flow'),
    'type' => 'flow',
    'entity' => 'task',
]
```
