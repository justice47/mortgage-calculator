<script setup lang="ts">
  import { ref } from 'vue'

  const emits = defineEmits<(eventName: 'select', value: boolean) => void>();

  const selectValue = ref('Yes')

  const onSelect = () => {
    if (selectValue.value === 'Yes') {
      emits('select', true)
    } else {
      emits('select', false)
    }
  }
</script>

<template>
  <div :class="$style.selectBox">
    <select
      v-model="selectValue"
      :class="$style.select"
      @change="onSelect"
    >
      <option value="Yes">Yes</option>
      <option value="No">No</option>
    </select>
  </div>
</template>

<style lang="scss" module>
  .selectBox {
    background-color: #F7F7F7;
    border: 1px solid #E0E0E0;
    cursor: pointer;
    border-radius: 4px;
    padding: 8px;
  }

  .select {
    background-color: transparent;
    font-size: 18px;
    cursor: pointer;
    outline: none;
    border: none;
    width: 100%;
  }
</style>