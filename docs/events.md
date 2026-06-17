# Events

All events emitted by the `<vue3-datatable>` component.

| Event              | Payload                              | Description                                                                                   |
| ------------------ | :----------------------------------- | --------------------------------------------------------------------------------------------- |
| **sortChange**     | `{ field, direction }`               | Fired when the sort column or direction changes.                                              |
| **searchChange**   | `string`                             | Fired when the global search value changes.                                                   |
| **pageChange**     | `number`                             | Fired when the current page changes.                                                          |
| **pageSizeChange** | `number`                             | Fired when the page size changes.                                                             |
| **rowSelect**      | `array<any>`                         | Fired when rows are selected or deselected via checkbox. Returns all currently selected rows. |
| **filterChange**   | `array<{ field, value, condition }>` | Fired when a column filter changes.                                                           |
| **rowClick**       | `any`                                | Fired when a row is clicked. Returns the row object.                                          |
| **rowDBClick**     | `any`                                | Fired when a row is double-clicked. Returns the row object.                                   |

## Examples

### Handling sort changes

```html
<template>
  <vue3-datatable :rows="rows" :columns="cols" @sortChange="onSortChange" />
</template>

<script setup lang="ts">
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'
  import { ref } from 'vue'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function onSortChange({ field, direction }: { field: string; direction: string }) {
    console.log(`Sorted by "${field}" in "${direction}" order`)
  }
</script>
```

### Handling page & page size changes

```html
<template>
  <vue3-datatable :rows="rows" :columns="cols" @pageChange="onPageChange" @pageSizeChange="onPageSizeChange" />
</template>

<script setup lang="ts">
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'
  import { ref } from 'vue'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function onPageChange(page: number) {
    console.log('Current page:', page)
  }

  function onPageSizeChange(size: number) {
    console.log('Rows per page:', size)
  }
</script>
```

### Row click & double click

```html
<template>
  <vue3-datatable :rows="rows" :columns="cols" @rowClick="onRowClick" @rowDBClick="onRowDBClick" />
</template>

<script setup lang="ts">
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'
  import { ref } from 'vue'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function onRowClick(row: any) {
    console.log('Clicked row:', row)
  }

  function onRowDBClick(row: any) {
    console.log('Double-clicked row:', row)
  }
</script>
```

### Row selection with checkboxes

```html
<template>
  <vue3-datatable :rows="rows" :columns="cols" :hasCheckbox="true" @rowSelect="onRowSelect" />
</template>

<script setup lang="ts">
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'
  import { ref } from 'vue'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function onRowSelect(selectedRows: any[]) {
    console.log('Selected rows:', selectedRows)
  }
</script>
```

### Column filter changes

```html
<template>
  <vue3-datatable :rows="rows" :columns="cols" :columnFilter="true" @filterChange="onFilterChange" />
</template>

<script setup lang="ts">
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'
  import { ref } from 'vue'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function onFilterChange(filters: { field: string; value: string; condition: string }[]) {
    console.log('Active filters:', filters)
  }
</script>
```

---

- [Props](./props.md)
- [Column Options](./columns.md)
- [Methods](./methods.md)
