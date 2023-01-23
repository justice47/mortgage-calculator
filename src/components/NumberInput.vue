
<script setup lang="ts">
  import { watch } from 'vue'
  import { useCurrencyInput, CurrencyDisplay } from 'vue-currency-input'
  import type { CurrencyInputOptions } from 'vue-currency-input'

  const props = defineProps<{
    options?: Partial<CurrencyInputOptions>;
    modelValue: number;
  }>();

  const standardOptions = {
    currency: 'EUR',
    precision: 0,
    valueRange: {
      min: 10000,
      max: 100000000
    },
    currencyDisplay: CurrencyDisplay.hidden,
    hideGroupingSeparatorOnFocus: false
  }

  const { inputRef, setValue } = useCurrencyInput({ ...standardOptions, ...props.options})

  watch(() => props.modelValue, value => {
      setValue(value)
    }
  )
</script>

<template>
  <input
    ref="inputRef"
    type="text"
    :class="$style.input"
  />
</template>

<style lang="scss" module>
  .input {
    background-color: transparent;
    font-size: 18px;
    line-height: 22px;
    outline: none;
    border: none;
  }
</style>