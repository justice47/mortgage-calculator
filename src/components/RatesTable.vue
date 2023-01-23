<script setup lang="ts">
  type RatesTableDataValue = {
    monthlyRate: number;
    borrowingRate: number;
  }

  type RatesTableData = {
    [key: string]: RatesTableDataValue;
  }

  defineProps<{
    data: RatesTableData;
  }>();
</script>

<template>
  <div :class="$style.ratesTable">
    <table>
      <tr>
        <th>
          Fixation length
        </th>

        <th>
          Monthly rate
        </th>

        <th>
          Interest rate
        </th>
      </tr>

      <tr
        v-for="(rate, years) in data"
        :key="years"
      >
        <td :class="$style.fixationLength">
          {{ years}} years
        </td>

        <td>
          {{ rate.monthlyRate }} €
        </td>

        <td>
          {{ rate.borrowingRate }} %
        </td>
      </tr>
    </table>
  </div>
</template>

<style lang="scss" module>
  .ratesTable {
    display: grid;
    text-align: left;
    row-gap: 8px;
    background-color: #ffffff;
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
    border-radius: 8px;
    padding: 16px 0;

    th {
      padding: 12px 24px;
    }

    td {
      padding: 12px 24px;
      border-top: 1px solid #e0e0e0;
    }
  }

  .fixationLength {
    font-weight: 700;
  }
</style>