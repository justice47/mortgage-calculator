<script setup lang="ts">
  import SpinnerCirlce from '@/components/icons/SpinnerCircle.vue';

  export interface Props {
    isLoading?: boolean
  }

  withDefaults(defineProps<Props>(), {
    isLoading: false
  })
</script>

<template>
  <button :class="$style.button">
    <SpinnerCirlce v-if="isLoading"/>

    <template v-else>
      Calculate
    </template>
  </button>
</template>

<style lang="scss" module>
  .button {
    display: grid;
    justify-content: center;
    align-content: center;
    min-width: 204px;
    height: 62px;
    position: relative;
    width: fit-content;
    background-color: #3E808F;
    cursor: pointer;
    padding: 16px 48px;
    border-radius: 4px;
    font-size: 18px;
    color: white;
    border: none;
  }
</style>
