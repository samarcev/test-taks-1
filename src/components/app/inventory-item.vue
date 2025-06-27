<script setup lang="ts">
defineProps({
  id: {
    type: Number,
    required: true
  },
  name: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="item">
    {{ name }}
  </div>
</template>

<style scoped lang="scss">
.item {
  width: 100px;
  height: 100px;
  background-color: #fff;
  border-radius: 5px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  font-size: 16px;
}
</style>
