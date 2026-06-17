# Usage

## Basic Example

```html
<template>
  <vue3-datatable :rows="rows" :columns="cols" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID', width: '90px', filter: false },
    { field: 'name', title: 'Name' },
    { field: 'username', title: 'Username' },
    { field: 'email', title: 'Email' },
    { field: 'phone', title: 'Phone' },
    { field: 'date', title: 'Date', type: 'date' },
    { field: 'active', title: 'Active', type: 'bool' },
    { field: 'age', title: 'Age', type: 'number' },
    { field: 'address.city', title: 'City' },
    { field: 'company.name', title: 'Company' },
  ])

  const rows = ref([
    {
      id: 1,
      name: 'Leanne Graham',
      username: 'Bret',
      email: 'Sincere@april.biz',
      address: {
        street: 'Kulas Light',
        suite: 'Apt. 556',
        city: 'Gwenborough',
        zipcode: '92998-3874',
        geo: { lat: '-37.3159', lng: '81.1496' },
      },
      phone: '1-770-736-8031 x56442',
      website: 'hildegard.org',
      company: {
        name: 'Romaguera-Crona',
        catchPhrase: 'Multi-layered client-server neural-net',
        bs: 'harness real-time e-markets',
      },
      date: 'Tue Sep 27 2022 22:19:57',
      age: 10,
      active: true,
    },
  ])
</script>
```

## Server-Side Pagination

Use `isServerMode` and `totalRows` when data is fetched from a backend:

```html
<template>
  <vue3-datatable
    :rows="rows"
    :columns="cols"
    :isServerMode="true"
    :totalRows="total"
    :page="page"
    :pageSize="pageSize"
    @pageChange="onPageChange"
    @pageSizeChange="onPageSizeChange"
  />
</template>

<script setup lang="ts">
  import { ref, onMounted } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
    { field: 'email', title: 'Email' },
  ])

  const rows = ref([])
  const total = ref(0)
  const page = ref(1)
  const pageSize = ref(10)

  async function fetchData() {
    const res = await fetch(`/api/users?page=${page.value}&limit=${pageSize.value}`)
    const data = await res.json()
    rows.value = data.items
    total.value = data.total
  }

  function onPageChange(newPage: number) {
    page.value = newPage
    fetchData()
  }

  function onPageSizeChange(newSize: number) {
    pageSize.value = newSize
    page.value = 1
    fetchData()
  }

  onMounted(fetchData)
</script>
```

## Global Search

Pass a reactive string to the `search` prop to filter rows across all columns:

```html
<template>
  <input v-model="search" placeholder="Search..." />
  <vue3-datatable :rows="rows" :columns="cols" :search="search" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const search = ref('')
  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
    { field: 'email', title: 'Email' },
  ])
  const rows = ref([
    /* ... */
  ])
</script>
```

## Checkboxes & Row Selection

Enable checkboxes with `hasCheckbox` and listen to the `rowSelect` event:

```html
<template>
  <vue3-datatable :rows="rows" :columns="cols" :hasCheckbox="true" @rowSelect="onRowSelect" />
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
  ])
  const rows = ref([
    /* ... */
  ])

  function onRowSelect(selectedRows: any[]) {
    console.log('Selected:', selectedRows)
  }
</script>
```

## Custom Cell Rendering

Use `cellRenderer` in a column definition to render custom HTML or a component:

```html
<template>
  <vue3-datatable :rows="rows" :columns="cols" />
</template>

<script setup lang="ts">
  import { ref, h } from 'vue'
  import Vue3Datatable from '@sebastiandotdev/vue3-datatable'
  import type { Column } from '@sebastiandotdev/vue3-datatable'
  import '@sebastiandotdev/vue3-datatable/dist/style.css'

  const cols = ref<Column[]>([
    { field: 'id', title: 'ID' },
    { field: 'name', title: 'Name' },
    {
      field: 'active',
      title: 'Status',
      cellRenderer: (row: any) =>
        h('span', { class: row.active ? 'text-green-500' : 'text-red-500' }, row.active ? 'Active' : 'Inactive'),
    },
  ])
  const rows = ref([
    /* ... */
  ])
</script>
```

## Accessing Methods via Template Ref

Use a template ref to call methods like `reset`, `getSelectedRows`, etc.:

```html
<template>
  <button @click="datatable?.reset()">Reset</button>
  <button @click="logSelected">Log Selected</button>
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
    console.log(datatable.value?.getSelectedRows())
  }
</script>
```

---

For all available props, column options, events, and methods refer to the following docs:

- [Props](./props.md)
- [Column Options](./columns.md)
- [Events](./events.md)
- [Methods](./methods.md)
