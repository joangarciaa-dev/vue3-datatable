<script setup lang="ts">
import type { ColumHeaderEmits, ColumnHeaderProps } from './types'
import { reactive, ref, watch } from 'vue'
import columnFilter from './column-filter.vue'
import iconCheck from './icon-check.vue'
import iconDash from './icon-dash.vue'
import iconFilter from './icon-filter.vue'

const props = defineProps<ColumnHeaderProps>()
const emit = defineEmits<ColumHeaderEmits>()
const selectedAll: any = ref(null)
const headerRefs = ref<(HTMLElement | null)[]>([])
const columnWidths = reactive<Record<number, string>>({})
const localOpenFilter = ref(props.isOpenFilter)

let resizeState = {
  isResizing: false,
  columnIndex: -1,
  startX: 0,
  startWidth: 0,
}

function setHeaderRef(el: any, index: number) {
  if (el) {
    headerRefs.value[index] = el
  }
}

function startResize(event: MouseEvent, columnIndex: number, _col: any) {
  event.preventDefault()
  event.stopPropagation()

  const header = headerRefs.value[columnIndex]
  if (!header)
    return

  resizeState = {
    isResizing: true,
    columnIndex,
    startX: event.pageX,
    startWidth: header.offsetWidth,
  }

  document.addEventListener('mousemove', handleResize)
  document.addEventListener('mouseup', stopResize)

  document.body.style.cursor = 'col-resize'
  document.body.style.userSelect = 'none'
}

function handleResize(event: MouseEvent) {
  if (!resizeState.isResizing)
    return

  const diff = event.pageX - resizeState.startX
  const newWidth = Math.max(10, resizeState.startWidth + diff)

  columnWidths[resizeState.columnIndex] = `${newWidth}px`
}

function stopResize() {
  resizeState.isResizing = false
  document.removeEventListener('mousemove', handleResize)
  document.removeEventListener('mouseup', stopResize)

  document.body.style.cursor = ''
  document.body.style.userSelect = ''
}

function checkboxChange() {
  if (selectedAll.value) {
    selectedAll.value.indeterminate
      = props.checkAll !== 0 ? !props.checkAll : false
    selectedAll.value.checked = props.checkAll
  }
}

watch(() => props.checkAll, checkboxChange)
</script>

<template>
  <tr key="hdrrow">
    <th
      v-if="props.all.hasCheckbox"
      key="chkall"
      class="bh-w-px"
      :class="{
        'bh-sticky bh-bg-blue-light bh-z-50':
          props.all.stickyHeader || props.all.stickyFirstColumn,
        'bh-top-0': props.all.stickyHeader,
        'bh-left-0': props.all.stickyFirstColumn,
      }"
    >
      <div class="bh-checkbox">
        <input
          ref="selectedAll"
          type="checkbox"
          @click.stop="
            emit('selectAll', ($event.target as HTMLInputElement)?.checked)
          "
        >
        <div>
          <icon-check class="check" />
          <icon-dash class="intermediate" />
        </div>
      </div>
    </th>
    <template v-for="(col, j) in props.all.columns">
      <th
        v-if="!col.hide"
        :key="col.field"
        :ref="(el) => setHeaderRef(el, j)"
        class="bh-select-none bh-z-[1] bh-relative bh-truncate"
        :class="[
          props.all.sortable && col.sort ? 'bh-cursor-pointer' : '',
          j === 0 && props.all.stickyFirstColumn
            ? 'bh-sticky bh-left-0 bh-bg-blue-light !bh-z-50'
            : '',
          props.all.hasCheckbox && j === 0 && props.all.stickyFirstColumn
            ? 'bh-left-[52px] !bh-z-50'
            : '',
          props.all.cellClass,
        ]"
        :style="{
          'width': columnWidths[j] || col.width,
          'min-width': columnWidths[j] || col.minWidth,
          'max-width': col.maxWidth,
        }"
      >
        <div
          class="bh-flex bh-items-center"
          :class="[col.headerClass ? col.headerClass : '']"
          @click="
            props.all.sortable && col.sort && emit('sortChange', col.field)
          "
        >
          {{ col.title }}
          <span
            v-if="props.all.sortable && col.sort"
            class="bh-ml-3 bh-sort bh-flex bh-items-center"
            :class="[props.currentSortColumn, props.currentSortDirection]"
          >
            <svg width="16" height="16" viewBox="0 0 14 14" fill="none">
              <polygon
                points="3.11,6.25 10.89,6.25 7,1.75 "
                fill="currentColor"
                class="bh-text-black/20"
                :class="[
                  currentSortColumn === col.field
                    && currentSortDirection === 'asc'
                    ? '!bh-text-primary'
                    : '',
                ]"
              />
              <polygon
                points="7,12.25 10.89,7.75 3.11,7.75 "
                fill="currentColor"
                class="bh-text-black/20"
                :class="[
                  currentSortColumn === col.field
                    && currentSortDirection === 'desc'
                    ? '!bh-text-primary'
                    : '',
                ]"
              />
            </svg>
          </span>
        </div>

        <template v-if="props.all.columnFilter && !props.isFooter">
          <div v-if="col.filter" class="bh-filter bh-relative">
            <input
              v-if="col.type === 'string'"
              v-model.trim="col.value"
              type="text"
              class="bh-form-control"
              @keyup="emit('filterChange')"
            >
            <input
              v-if="col.type === 'number'"
              v-model.number.trim="col.value"
              type="number"
              class="bh-form-control"
              @keyup="emit('filterChange')"
            >
            <input
              v-else-if="col.type === 'date'"
              v-model="col.value"
              type="date"
              class="bh-form-control"
              @change="emit('filterChange')"
            >
            <select
              v-else-if="col.type === 'bool'"
              v-model="col.value"
              class="bh-form-control"
              @change="emit('filterChange')"
              @click="localOpenFilter = null"
            >
              <option :value="undefined">
                All
              </option>
              <option :value="true">
                True
              </option>
              <option :value="false">
                False
              </option>
            </select>

            <button
              v-if="col.type !== 'bool'"
              type="button"
              @click.stop="emit('toggleFilterMenu', col)"
            >
              <icon-filter class="bh-w-4" />
            </button>

            <column-filter
              v-show="localOpenFilter === col.field"
              :column="col"
              :type="col.type"
              :column-filter-lang="props.columnFilterLang"
              @close="emit('toggleFilterMenu', null)"
              @filter-change="(updated) => { Object.assign(col, updated); emit('filterChange') }"
            />
          </div>
        </template>

        <div
          v-if="props.all.resizeEnabled"
          class="bh-absolute bh-top-0 bh-right-0 bh-w-[2px] bh-h-1/2 bh-translate-y-1/2 bh-cursor-col-resize bh-z-10 bh-bg-gray-400 active:bh-bg-primary bh-rounded-full"
          @mousedown="startResize($event, j, col)"
        />
      </th>
    </template>
  </tr>
</template>
