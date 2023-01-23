<script setup lang="ts">
  import { ref, computed } from 'vue';
  import { useLazyQuery } from '@vue/apollo-composable'
  import gql from 'graphql-tag'
  import InputTitle from '@/components/InputTitle.vue';
  import AmountInput from '@/components/AmountInput.vue';
  import PercentageInput from '@/components/PercentageInput.vue'
  import RatesTable from '@/components/RatesTable.vue';
  import DefaultButton from '@/components/DefaultButton.vue';
  import DefaultSelect from '@/components/DefaultSelect.vue';
  import HorizontalDivider from '@/components/HorizontalDivider.vue';

  const brokerTax = 0.0714;
  const cityTax = 0.06;

  const propertyPrice = ref(128000);
  const totalSavings = ref(25500);
  const estateHasComission = ref(true);
  const repaymentRatePercentage = ref(2.7);

  const totalSavingsMax = computed(() : number => {
    return propertyPrice.value * 0.9
  })

  const propertyPriceMin = computed(() : number => {
    return totalSavings.value * 1.2
  })

  const notaryCosts = computed(() : number => {
    return 2144 + (0.013 * (propertyPrice.value - 100000.0))
  })

  const brokerCosts = computed(() : number => {
    return estateHasComission.value ? brokerTax * propertyPrice.value : 0;
  })

  const stampDutyCosts = computed(() : number => {
    return cityTax * propertyPrice.value
  })

  const totalCost = computed(() : number => {
    return notaryCosts.value + brokerCosts.value + stampDutyCosts.value
  })

  const impliedLoan = computed(() : number | string => {
    if (propertyPrice.value !== null && totalSavings.value !== null) {
      return totalCost.value - totalSavings.value + propertyPrice.value
    } else {
      return ''
    }
  })

  const impliedLoanFormatted = computed(() : string => {
    if (typeof impliedLoan.value === 'number') {
      return Math.floor(impliedLoan.value).toLocaleString();
    } else return ''
  })

  const loanToValue = computed(() : number | string => {
    if (typeof impliedLoan.value === 'number') {
      return (impliedLoan.value / propertyPrice.value * 100)
    } else return ''
  })

  const loanToValueFormatted = computed(() : string => {
    if (typeof loanToValue.value === 'number') {
      return loanToValue.value.toFixed(2)
    } else return ''
  })

  const { result, variables, load } = useLazyQuery(gql`
    query ratesTable($propetyPrice: Float!, $repayment: Float!, $loanAmount: Float!) {
      root {
        rates_table(
          property_price: $propetyPrice,
          repayment: $repayment,
          loan_amount: $loanAmount,
          years_fixed: [5,10,15,20,25,30]
        )
      }
    }
  `, {
    propetyPrice: propertyPrice.value,
    repayment: repaymentRatePercentage.value,
    loanAmount: impliedLoan.value
  })

  const selectHandler = (selectValue: boolean) : void => {
    estateHasComission.value = selectValue;
  }

  const calculateHandler = () : void => {
    variables.value = {
      propetyPrice: propertyPrice.value,
      repayment: repaymentRatePercentage.value,
      loanAmount: impliedLoan.value
    }

    load()
  }
</script>

<template>
  <main>
    <div :class="$style.calculator">
      <div :class="$style.mainCalculations">
        <div :class="$style.options">
          <h1 :class="$style.calculatorTitle">Mortgage Calculator</h1>

          <div :class="$style.inputs">
            <div>
              <InputTitle title="Property price input" />

              <AmountInput
                v-model.lazy="propertyPrice"
                :options="{
                  valueRange: { min: propertyPriceMin, max: 100000000 }
                }"
              />
            </div>

            <div>
              <InputTitle title="Total savings input" />

              <AmountInput
                v-model.lazy="totalSavings"
                :options="{
                  valueRange: { min: 500, max: totalSavingsMax }
                }"
              />
            </div>

            <div>
              <InputTitle title="Real estate comission" />

              <DefaultSelect @select="selectHandler"/>
            </div>

            <div>
              <InputTitle title="Annual repayment rate (%)" />

              <PercentageInput v-model="repaymentRatePercentage"/>
            </div>
          </div>

          <HorizontalDivider />

          <div :class="$style.controls">
            <DefaultButton @click="calculateHandler">
              Calculate
            </DefaultButton>
          </div>
        </div>

        <div :class="$style.results">
          <div :class="$style.impliedLoan">
            Implied loan

            <span :class="$style.loanResult">
              {{ impliedLoanFormatted }} €
            </span>
          </div>

          <div :class="$style.loanToValue">
            Loan to value

            <span :class="$style.loanResult">
              {{ loanToValueFormatted }} %
            </span>
          </div>
        </div>
      </div>

      <Transition>
        <RatesTable
          v-if="result"
          :data="result.root.ratesTable"
        />
      </Transition>
    </div>
  </main>
</template>

<style lang="scss" module>
  @import "@/styles/mixins";

  .calculator {
    display: grid;
    row-gap: 16px;
  }

  .mainCalculations {
    display: grid;
    row-gap: 16px;
  }

  .calculatorTitle {
    font-weight: 500;
    font-size: 24px;
    padding: 24px 24px 0;
  }

  .options {
    @include panel();

    display: grid;
    row-gap: 16px;
  }

  .inputs {
    padding: 0 24px;
    display: grid;
    gap: 24px;
    grid-template-columns: auto auto;
    grid-template-rows: auto auto;

    @include mobile() {
      grid-template-columns: 1fr;
      grid-template-rows: auto auto auto auto;
    }
  }

  .controls {
    display: grid;
    justify-content: end;
    padding: 0 24px 24px;

    @include mobile() {
      justify-content: center;
    }
  }

  .results {
    display: grid;
    grid-auto-flow: column;
    grid-template-columns: 1fr 1fr;
    gap: 16px;

    @include mobile() {
      grid-auto-flow: row;
      grid-template-columns: unset;
    }
  }

  .impliedLoan {
    @include panel();

    display: grid;
    row-gap: 8px;
    padding: 24px;
  }

  .loanToValue {
    @include panel();

    display: grid;
    row-gap: 8px;
    padding: 24px;
  }

  .loanResult {
    font-weight: 500;
    font-size: 24px;
    min-height: 40px;
  }

  .v-enter-active,
  .v-leave-active {
    transition: opacity 0.5s ease;
  }

  .v-enter-from,
  .v-leave-to {
    opacity: 0;
  }
</style>
