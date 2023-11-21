<template>
  <form @submit.prevent="calculateTaxes" class="box-border  p-1 flex flex-col md:justify-start content-center justify-center border rounded gap-2">
    <input required v-model="payValue" class="outline border rounded"  placeholder="Введіть суму доходів" type="number">
    <div class="flex gap-3">
      <div class="flex gap-1">
        <input required id="third" name="interestRate" v-model="selectedRate" type="radio" value=0.03>
        <label for="third">3%</label>
      </div>
      <div class="flex gap-1">
        <input required id="five" name="interestRate" v-model="selectedRate" type="radio" value=0.05>
        <label for="five">5%</label>
      </div>
    </div>

    <div class="flex gap-3">
      <div class="flex gap-1">
        <input required id="oneMounts" name="period" v-model="period" type="radio" value=1>
        <label for="oneMounts">1 місяць</label>
      </div>
      <div class="flex gap-1">
        <input required id="threeMounts" name="period" v-model="period" type="radio" value=3>
        <label for="threeMounts">3 місяці</label>
      </div>
    </div>
    <div v-if="infoVisible">
      <p>Сума податків: {{ taxSum }}</p>
      <p>З яких<br>ЄСВ: {{ esv }}<br>ЄП: {{ fixedTax }}<br>Залишок: {{ saldo }}</p>
    </div>

    <button type="submit" class="border rounded self-center" >Підрахувати податки</button>
    <button type="button" @click="clearForm">Очистити</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';

const payValue = ref(null)
const taxSum = ref(0)
const esv = ref(1474)
const fixedTax = ref(0)
const saldo = ref(0)
const infoVisible = ref(false)
const selectedRate = ref(0.05)
const period = ref(1)

const calculateTaxes = () => {
  fixedTax.value = (payValue.value * selectedRate.value) * period.value
  taxSum.value = (fixedTax.value + esv.value) * period.value
  saldo.value = (payValue.value - esv.value - fixedTax.value) * period.value
  infoVisible.value = true
}

const clearForm = () => {
  infoVisible.value = false
  payValue.value = null
  taxSum.value = 0
  fixedTax.value = 0
  saldo.value = 0
  selectedRate.value = null
  period.value = null
}
</script>