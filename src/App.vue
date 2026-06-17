<script setup lang="ts">
import type {
  Column,
  Vue3DatatableChangeEvent,
  Vue3DatatableExposedMethods,
} from './components'
import { onMounted, reactive, ref } from 'vue'
import Vue3Datatable from './components'
import '../dist/style.css'

const datatable = ref<Vue3DatatableExposedMethods>()
const loading = ref(true)
const total_rows = ref(0)
const rows = ref<Array<any>>([])
const cols = ref<Column[]>([
  { field: 'id', title: 'ID', isUnique: true, filter: false },
  { field: 'firstName', title: 'First Name' },
  { field: 'lastName', title: 'Last Name' },
  { field: 'email', title: 'Email' },
  { field: 'age', title: 'Age', type: 'number' },
  { field: 'dob', title: 'Birthdate', type: 'date' },
  { field: 'address.city', title: 'City' },
  { field: 'isActive', title: 'Active', type: 'bool' },
]) || []
const params = reactive<Partial<Vue3DatatableChangeEvent>>({
  current_page: 1,
  pagesize: 5,
  sort_column: 'id',
  sort_direction: 'desc',
  column_filters: [],
  search: '',
})

let controller: AbortController | null = null
let timer: NodeJS.Timeout | null = null

function changeServer(data: Vue3DatatableChangeEvent) {
  params.current_page = data.current_page
  params.pagesize = data.pagesize
  params.sort_column = data.sort_column || 'id'
  params.sort_direction = data.sort_direction
  params.column_filters = data.column_filters
  params.search = data.search

  filterUsers()
}

function filterUsers() {
  if (timer)
    clearTimeout(timer)
  timer = setTimeout(() => {
    getUsers()
  }, 300)
}

async function getUsers() {
  // cancel request if previous request still pending before next request fire
  if (controller) {
    controller.abort()
  }
  controller = new AbortController()
  const signal = controller.signal

  try {
    loading.value = true

    const response = await fetch(
      'https://vue3-datatable-document.vercel.app/api/user',
      {
        method: 'POST',
        body: JSON.stringify(params),
        signal,
      },
    )

    const data = await response.json()

    rows.value = data?.data
    total_rows.value = data?.meta?.total
  }
  catch {}

  loading.value = false
}

onMounted(() => {
  getUsers()
})
</script>

<template>
  <div class="bh-p-10">
    <div class="bh-mb-2">
      <input
        v-model="params.search"
        type="text"
        placeholder="Search..."
        class="bh-border bh-border-solid bh-bg-white bh-p-2 bh-outline-0 bh-border-gray-200 focus:bh-border-gray-200 bh-rounded"
      >
      <button type="button" class="btn mb-4 bh-p-2" @click="datatable?.reset()">
        Reset
      </button>
      <br>
    </div>
    <Vue3Datatable
      ref="datatable"
      :loading="loading"
      :rows="rows"
      :columns="cols"
      :total-rows="total_rows"
      :is-server-mode="true"
      :page="params.current_page"
      :page-size="params.pagesize"
      :page-size-options="[3, 5, 10]"
      :sortable="true"
      :sort-column="params.sort_column"
      :sort-direction="params.sort_direction"
      :search="params.search"
      :has-checkbox="true"
      :column-filter="false"
      :resize-enabled="true"
      :sticky-first-column="true"
      :sticky-header="true"
      class="datatable"
      @change="changeServer"
    />
  </div>
</template>
