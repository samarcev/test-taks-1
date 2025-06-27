<script setup lang="ts">
import InventoryItem from '@/components/app/inventory-item.vue'
import type { InventoryItemInterface } from '@/shared/interfaces/inventory-item.ts'
import { type PropType, ref } from 'vue'

const { multi, items } = defineProps({
  multi: {
    type: Boolean,
    default: false,
  },
  items: {
    type: Array as PropType<InventoryItemInterface[]>,
    default: () => [],
  },
  maxSelectItems: {
    type: Number,
    default: null,
  },
})
const emit = defineEmits(['onSelect'])
const selectedItems = ref<number[]>([])
const onChange = (event: Event, item: number): void => {
  const input = event.target as HTMLInputElement
  handleSelect(item, input.checked)
}

const handleSelect = (item: number, isChecked: boolean) => {
  if (multi) {
    if (isChecked) {
      selectedItems.value.push(item)
    } else {
      selectedItems.value = selectedItems.value.filter((el) => el !== item)
    }
  } else {
    selectedItems.value = [item]
  }

  const result = multi
    ? selectedItems.value.map((id) => {
        return items.find((i) => i.id === id)
      })
    : items.find((i) => selectedItems.value.includes(i.id))

  emit('onSelect', result)
}
</script>

<template>
  <div class="inventory">
    <label v-for="item in items" :key="item.id" class="inventory-item">
      <input
        :type="multi ? 'checkbox' : 'radio'"
        :checked="selectedItems.includes(item.id)"
        :disabled="
          maxSelectItems
            ? selectedItems.length >= maxSelectItems && !selectedItems.includes(item.id)
            : false
        "
        @change="(e) => onChange(e, item.id)"
      />
      <inventory-item :id="item.id" :name="item.name" />
    </label>
  </div>
</template>

<style scoped lang="scss">
.inventory {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px;
  &-item {
    position: relative;
    cursor: pointer;
    input {
      position: absolute;
      top: 0;
      right: 0;
      left: 0;
      bottom: 0;
      z-index: -100;
      opacity: 0;
      visibility: hidden;
      &:checked {
        & + div {
          background-color: #d9d9d9;
          border: 2px solid #000;
        }
      }
    }
    &:has(input:disabled) {
      cursor: not-allowed;
      filter: grayscale(0);
      opacity: 0.5;
    }
  }
}
</style>
