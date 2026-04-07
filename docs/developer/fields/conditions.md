# Conditions Field (`type: conditions`)

Grid-based condition builder with source/key/operator/compare columns.

## Typical Props

- `taggable`
- `sortable`
- `conditionTypes`
- optional `source` config, options correspond with the `select` field. This param is meant to support other data sources to compare with.

## Example

```php
'conditions' => [
    'label' => $this->trans('Conditions'),
    'type' => 'conditions',
    'sortable' => true,
]
```

## Additional Examples

```php
'conditions' => [
    'type' => 'conditions',
    'sortable' => true,
    'default' => [
        [
            'source' => '{{ data }}',
            'key' => 'status',
            'operator' => '==',
            'compare' => 'active'
        ],
    ],
]
```

```php
'conditions' => [
    'type' => 'conditions',
	'source'   => [
		'placeholder' => '{{ row }}', // Just UI as a default
		'customizable' => true,
		'choices' => [
			'' => 'Row', // Like shown in the paceholder, this field defaults to the "Row" data.
			'{{ data }}' => 'Data',
			'{{ cache }}' => 'Cache',
			'{{ variables }}' => 'Variables',
		],
	],
    'taggable' => true,
]
```
