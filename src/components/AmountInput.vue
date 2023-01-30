<script setup lang="ts">
  import { ref, watch } from 'vue'
  import { CurrencyDisplay } from 'vue-currency-input'
  import type { CurrencyInputOptions } from 'vue-currency-input'
  import NumberInput from '@/components/NumberInput.vue'

  const props = defineProps<{
    options?: Partial<CurrencyInputOptions>;
    modelValue: number;
  }>();

  const emits = defineEmits<(eventName: 'update:modelValue', value: number) => void>();

  const amount = ref(props.modelValue)

  watch(amount, () : void => {
    emits('update:modelValue', amount.value) 
  })
</script>

<template>
  <div :class="$style.inputBox">
    <span :class="$style.prefix">€</span>

    <NumberInput v-model.lazy="amount" />
  </div>
</template>

<style lang="scss" module>
  .inputBox {
    display: grid;
    grid-auto-flow: column;
    justify-content: start;
    column-gap: 4px;
    background-color: #F7F7F7;
    border: 1px solid #E0E0E0;
    border-radius: 4px;
    font-size: 16px;
    padding: 8px;
  }

  .input {
    background-color: transparent;
    font-size: 18px;
    outline: none;
    border: none;
  }

  .input::-webkit-outer-spin-button,
  .input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
</style>