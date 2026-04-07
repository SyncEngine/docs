# Field Reference

This page is the developer index for all field types used in SyncEngine model `getFields()` definitions.

## Start Here

- [General Properties](fields/general-properties.md): Shared options, structural props, wrappers, and nesting behavior.

## Field Types

- [Authentication](fields/authentication.md)
- [Code](fields/code.md)
- [Codec](fields/codec.md)
- [Column](fields/column.md)
- [Conditions](fields/conditions.md)
- [Dataset](fields/dataset.md)
- [Entities](fields/entities.md)
- [Entity](fields/entity.md)
- [Flow](fields/flow.md)
- [Grid](fields/grid.md)
- [HTML](fields/html.md)
- [Input](fields/input.md)
- [Mapper](fields/mapper.md)
- [Params](fields/params.md)
- [Radio](fields/radio.md)
- [Repeater](fields/repeater.md)
- [Schema](fields/schema.md)
- [Secret](fields/secret.md)
- [Select](fields/select.md)
- [Separator](fields/separator.md)
- [Sequence](fields/sequence.md)
- [Tasks](fields/tasks.md)
- [Textarea](fields/textarea.md)
- [Title](fields/title.md)
- [Toggle](fields/toggle.md)
- [Webservice](fields/webservice.md)

## Aliases

- `Boolean`: use [Toggle](fields/toggle.md)
- `Checkbox`: use [Toggle](fields/toggle.md)
- `Switch`: use [Toggle](fields/toggle.md)

## Notes

- In PHP field arrays, `type: text` falls back to the `Input` field.
- React aliases:
  - `textarea` -> `input` with multiline enabled
  - `boolean`, `checkbox`, and `switch` -> `toggle`
- UI helper field types: `title`, `separator`, `html`.