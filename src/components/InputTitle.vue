<script setup lang="ts">
  defineProps<{
    title: String;
  }>()
</script>

<template>
  <div :class="$style.title">
    {{ title }}
  </div>
</template>

<style lang="scss" module>
  .title {
    font-size: 16px;
  }
</style>
