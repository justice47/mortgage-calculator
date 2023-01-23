<script setup lang="ts">
  import { ref, watch } from 'vue'
  import { CurrencyDisplay } from 'vue-currency-input'
  import NumberInput from '@/components/NumberInput.vue'
  import MinusIcon from '@/components/icons/MinusIcon.vue'
  import PlusIcon from '@/components/icons/PlusIcon.vue'

  const props = defineProps<{modelValue: number;}>();

  const emits = defineEmits<(eventName: 'update:modelValue', value: number) => void>();

  const numberInputOptions = {
    currency: 'EUR',
    precision: 1,
    valueRange: {
      min: 0.1,
      max: 20
    },
    currencyDisplay: CurrencyDisplay.hidden
  }

  const repaymentRate = ref(props.modelValue)

  watch(repaymentRate, () : void => {
    emits('update:modelValue', repaymentRate.value) 
  })

  const decreaseRepaymentRate = () : void => {
    repaymentRate.value = repaymentRate.value - 0.1
  }

  const increaseRepaymentRate = () : void => {
    repaymentRate.value = repaymentRate.value + 0.1
  }
</script>

<template>
  <div :class="$style.inputBox">
    <div :class="$style.prefix">
      <MinusIcon
        :class="$style.icon"
        @click="decreaseRepaymentRate"
      />
    </div>

    <NumberInput
      v-model="repaymentRate"
      :options="numberInputOptions"
      :class="$style.percentageInput"
    />

    <div :class="$style.postifx">
      <PlusIcon
        :class="$style.icon"
        @click="increaseRepaymentRate"
      />
    </div>
  </div>
</template>

<style lang="scss" module>
  .inputBox {
    display: grid;
    grid-auto-flow: column;
    grid-template-columns: auto 1fr auto;
    background-color: #F7F7F7;
    border: 1px solid #E0E0E0;
    border-radius: 4px;
    font-size: 16px;
    line-height: 22px;
    column-gap: 4px;
    padding: 8px;
  }

  .prefix {
    display: grid;
    align-content: center;
  }

  .icon {
    cursor: pointer;
    width: 16px;
    height: 16px;
  }

  .percentageInput {
    text-align: center;
  }
</style>