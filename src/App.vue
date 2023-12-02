<template>
  <Header />
  <div class="container">
    <Balance :totalBalance="totalBalance" />
    <IncomeExpenses :incomes="incomes" :expenses="expenses" />
    <transaction-list :transactions="transactions" />
    <add-transaction @transactionDataSubmitted="transactionDataSubmitted" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue"
import Balance from "./components/Balance.vue"
import IncomeExpenses from "./components/IncomeExpenses.vue"
import TransactionList from "./components/TransactionList.vue"
import AddTransaction from "./components/AddTransaction.vue"

import { ref, computed } from "vue"

const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 299.97 },
  { id: 3, text: "Movie", amount: -10.0 },
])

// get balance
const totalBalance = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

// get income
const incomes = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    if (transaction.amount > 0) {
      return acc + transaction.amount
    }
    return acc
  }, 0)
})

// get expenses
const expenses = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    if (transaction.amount < 0) {
      return acc + transaction.amount * -1
    }
    return acc
  }, 0)
})

const transactionDataSubmitted = (data) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: data.text,
    amount: data.amount,
  })
  console.log(generateUniqueId())
}

function generateUniqueId() {
  return Math.floor(Math.random() * 10000000)
}
</script>
