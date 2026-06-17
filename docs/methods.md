# Methods

Methods exposed by the `<vue3-datatable>` component and accessible via a template ref.

## Setup

To call methods, first obtain a reference to the component instance using `ref`:

```html
<template>
  <vue3-datatable ref="datatable" :rows="rows" :columns="cols" :hasCheckbox="true" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const datatable = ref<InstanceType<typeof Vue3Datatable> | null>(null)
  const cols = ref<Column[]>([
    /* ... */
  ])
  const rows = ref([
    /* ... */
  ])
</script>
```

---

## Reference

| Method                   | Returns         | Description                                                                         |
| ------------------------ | :-------------- | ----------------------------------------------------------------------------------- |
| **reset()**              | `void`          | Resets all table state: selected rows, column filters, global search, current page. |
| **getFilteredRows()**    | `array<any>`    | Returns all rows that match the current filters and search query.                   |
| **getColumnFilters()**   | `array<object>` | Returns the current active column filters.                                          |
| **getSelectedRows()**    | `array<any>`    | Returns all currently selected rows.                                                |
| **clearSelectedRows()**  | `void`          | Deselects all selected rows.                                                        |
| **selectRow(index)**     | `void`          | Selects the row at the given index. Non-existent indexes are silently ignored.      |
| **unselectRow(index)**   | `void`          | Deselects the row at the given index. Non-existent indexes are silently ignored.    |
| **isRowSelected(index)** | `boolean`       | Returns `true` if the row at the given index is currently selected.                 |

---

## Examples

### reset()

Resets the entire table state back to its initial values (page 1, no filters, no selection, empty search):

```html
<template>
  <button @click="datatable?.reset()">Reset Table</button>
  <vue3-datatable ref="datatable" :rows="rows" :columns="cols" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const datatable = ref<InstanceType<typeof Vue3Datatable> | null>(null)
  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])
</script>
```

---

### getFilteredRows()

Returns the rows currently visible after applying all active filters and the global search:

```html
<template>
  <button @click="logFiltered">Log Filtered Rows</button>
  <vue3-datatable ref="datatable" :rows="rows" :columns="cols" :columnFilter="true" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const datatable = ref<InstanceType<typeof Vue3Datatable> | null>(null)
  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function logFiltered() {
    const filtered = datatable.value?.getFilteredRows()
    console.log('Filtered rows:', filtered)
  }
</script>
```

---

### getColumnFilters()

Returns the list of currently active column filters:

```html
<template>
  <button @click="logFilters">Log Column Filters</button>
  <vue3-datatable ref="datatable" :rows="rows" :columns="cols" :columnFilter="true" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const datatable = ref<InstanceType<typeof Vue3Datatable> | null>(null)
  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function logFilters() {
    const filters = datatable.value?.getColumnFilters()
    console.log('Active column filters:', filters)
  }
</script>
```

---

### getSelectedRows() & clearSelectedRows()

Retrieve or clear the currently selected rows:

```html
<template>
  <button @click="logSelected">Log Selected</button>
  <button @click="datatable?.clearSelectedRows()">Clear Selection</button>
  <vue3-datatable ref="datatable" :rows="rows" :columns="cols" :hasCheckbox="true" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const datatable = ref<InstanceType<typeof Vue3Datatable> | null>(null)
  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function logSelected() {
    const selected = datatable.value?.getSelectedRows()
    console.log('Selected rows:', selected)
  }
</script>
```

---

### selectRow(index) & unselectRow(index)

Programmatically select or deselect a row by its index:

```html
<template>
  <button @click="datatable?.selectRow(0)">Select First Row</button>
  <button @click="datatable?.unselectRow(0)">Deselect First Row</button>
  <vue3-datatable ref="datatable" :rows="rows" :columns="cols" :hasCheckbox="true" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const datatable = ref<InstanceType<typeof Vue3Datatable> | null>(null)
  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])
</script>
```

---

### isRowSelected(index)

Check whether a specific row is currently selected:

```html
<template>
  <button @click="checkFirstRow">Is First Row Selected?</button>
  <vue3-datatable ref="datatable" :rows="rows" :columns="cols" :hasCheckbox="true" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const datatable = ref<InstanceType<typeof Vue3Datatable> | null>(null)
  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function checkFirstRow() {
    const selected = datatable.value?.isRowSelected(0)
    console.log('Is row 0 selected?', selected)
  }
</script>
```

---

- [Props](./props.md)
- [Column Options](./columns.md)
- [Events](./events.md)
