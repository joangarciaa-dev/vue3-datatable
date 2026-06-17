# @sebastiandotdev/vue3-datatable

A lightweight and flexible data table component for Vue 3 with sorting, filtering, pagination, and more.

## Demo

[Documentation & Demos](https://vue3-datatable-document.vercel.app)

## Install

#### npm

```bash
npm install @sebastiandotdev/vue3-datatable
```

#### yarn

```bash
yarn add @sebastiandotdev/vue3-datatable
```

#### pnpm

```bash
pnpm add @sebastiandotdev/vue3-datatable
```

## Quick Start

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
    { field: 'email', title: 'Email' },
    { field: 'date', title: 'Date', type: 'date' },
    { field: 'active', title: 'Active', type: 'bool' },
    { field: 'age', title: 'Age', type: 'number' },
  ])

  const rows = ref([
    {
      id: 1,
      name: 'Leanne Graham',
      email: 'Sincere@april.biz',
      date: 'Tue Sep 27 2022 22:19:57',
      age: 10,
      active: true,
    },
  ])
</script>
```

## Documentation

- [Usage & Examples](./docs/usage.md)
- [Props](./docs/props.md)
- [Column Options](./docs/columns.md)
- [Events](./docs/events.md)
- [Methods](./docs/methods.md)

## License

MIT — this project is a fork of [**@bhplugin/vue3-datatable**](https://github.com/bhaveshpatel200/vue3-datatable) by [Bhavesh Patel](https://github.com/bhaveshpatel200), licensed under the [MIT license](http://opensource.org/licenses/MIT).

See the [LICENSE](./LICENSE.md) file for details.
