<script setup lang="ts">
import InventoryCase from '@/components/app/inventory-case.vue'
import InventoryItem from '@/components/app/inventory-item.vue'
import type { InventoryItemInterface } from '@/shared/interfaces/inventory-item.ts'
import { useStore } from '@/stores'
import { ref } from 'vue'

const maxUserSelectedItems = 6
const store = useStore()

const userSelected = ref<InventoryItemInterface[]>([])
const shopSelected = ref<InventoryItemInterface | null>(null)
</script>

<template>
  <main>
    <div id="user-inventory" class="container">
      <div id="user-selected-items" class="selected-items">
        <div class="cart">
          <div v-for="item in userSelected" :key="item.id">
            <inventory-item :id="item.id" :name="item.name" />
          </div>
        </div>
        <span class="selected-items-counter">
          {{ userSelected.length }} / {{ maxUserSelectedItems }}
        </span>
      </div>
      <inventory-case
        multi
        :items="store.userItems"
        :max-select-items="maxUserSelectedItems"
        @onSelect="
          (items) => {
            userSelected = items
          }
        "
      />
    </div>
    <div id="shop-inventory" class="container">
      <div id="shop-selected-item" class="selected-items">
        <div class="cart">
          <div class="cart-item" v-if="shopSelected">
            {{ shopSelected.name }}
          </div>
        </div>
      </div>
      <inventory-case :items="store.shopItems" @onSelect="(items) => {
        shopSelected = items
      }" />
    </div>
  </main>
</template>

<style scoped>
main {
  min-height: 100vh;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 100px;
  .container {
    display: flex;
    flex-direction: column;
    gap: 50px;
    padding: 30px;
    flex: 1;
  }
  .selected-items {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 200px;
    overflow: auto;
    gap: 20px;
  }
  .cart {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    .cart-item {
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 30px;
      font-size: 32px;
      background-color: #fff;
      border-radius: 5px;
    }
  }
}
</style>
