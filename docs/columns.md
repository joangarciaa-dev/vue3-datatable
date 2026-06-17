# Column Options

All available options for each column definition passed to the `columns` prop.

| Option           | Type                   | Default     | Description                                                                                             |
| ---------------- | :--------------------- | ----------- | ------------------------------------------------------------------------------------------------------- |
| **field**        | `string`               | `""`        | Key of the data property to display. Supports dot notation for nested objects (e.g. `"address.city"`).  |
| **title**        | `string`               | `""`        | Label shown in the column header.                                                                       |
| **isUnique**     | `boolean`              | `false`     | Mark this column as the primary key / unique identifier.                                                |
| **value**        | `string`               | `""`        | Pre-set filter value for this column when column filtering is enabled.                                  |
| **condition**    | `string`               | `"contain"` | Default filter condition when column filtering is enabled. See [Filter Conditions](#filter-conditions). |
| **type**         | `string`               | `""`        | Data type of the column. Accepted values: `"string"`, `"date"`, `"number"`, `"bool"`.                   |
| **width**        | `string`               | `""`        | Fixed width of the column (e.g. `"120px"`, `"10%"`).                                                    |
| **minWidth**     | `string`               | `""`        | Minimum width of the column (e.g. `"80px"`).                                                            |
| **maxWidth**     | `string`               | `""`        | Maximum width of the column (e.g. `"300px"`).                                                           |
| **hide**         | `boolean`              | `false`     | Hide the column from the table.                                                                         |
| **filter**       | `boolean`              | `true`      | Include this column in the per-column filter row.                                                       |
| **search**       | `boolean`              | `true`      | Include this column in the global search.                                                               |
| **sort**         | `boolean`              | `true`      | Allow sorting by this column.                                                                           |
| **cellRenderer** | `function` \| `string` | `undefined` | Custom cell renderer. Accepts an HTML string or a render function `(row) => VNode`.                     |
| **headerClass**  | `string`               | `""`        | Extra CSS class(es) applied to the column's header cell.                                                |
| **cellClass**    | `string`               | `""`        | Extra CSS class(es) applied to every data cell in this column.                                          |

## Filter Conditions

Available values for the `condition` option (used when `columnFilter` is enabled on the table):

| Value                | Description                    |
| -------------------- | ------------------------------ |
| `contain`            | Cell value contains the input  |
| `not_contain`        | Cell value does not contain    |
| `equal`              | Exact match                    |
| `not_equal`          | Does not match                 |
| `start_with`         | Cell value starts with         |
| `end_with`           | Cell value ends with           |
| `greater_than`       | Greater than (numbers / dates) |
| `greater_than_equal` | Greater than or equal          |
| `less_than`          | Less than (numbers / dates)    |
| `less_than_equal`    | Less than or equal             |
| `is_null`            | Value is null / empty          |
| `is_not_null`        | Value is not null / empty      |

## Examples

### Basic column

```ts
{ field: "name", title: "Full Name" }
```

### Nested object field

```ts
{ field: "address.city", title: "City" }
```

### Typed columns

```ts
{ field: "createdAt", title: "Created", type: "date" }
{ field: "age",       title: "Age",     type: "number" }
{ field: "active",    title: "Active",  type: "bool" }
```

### Fixed width with filter disabled

```ts
{ field: "id", title: "ID", width: "80px", filter: false }
```

### Custom cell renderer (render function)

```ts
import { h } from "vue";

{
  field: "status",
  title: "Status",
  cellRenderer: (row) =>
    h(
      "span",
      { class: row.active ? "text-green-600" : "text-red-500" },
      row.active ? "Active" : "Inactive"
    ),
}
```

### Custom cell renderer (HTML string)

```ts
{
  field: "actions",
  title: "Actions",
  sort: false,
  filter: false,
  cellRenderer: () => `<button class="btn-edit">Edit</button>`,
}
```

---

- [Props](./props.md)
- [Events](./events.md)
- [Methods](./methods.md)
