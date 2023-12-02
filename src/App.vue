<template>
  <Header />
  <div class="container">
    <Balance :totalBalance="totalBalance" />
    <IncomeExpenses :incomes="incomes" :expenses="expenses" />
    <transaction-list
      :transactions="transactions"
      @transactionDeleted="deleteTransaction"
    />
    <add-transaction @transactionDataSubmitted="transactionDataSubmitted" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue"
import Balance from "./components/Balance.vue"
import IncomeExpenses from "./components/IncomeExpenses.vue"
import TransactionList from "./components/TransactionList.vue"
import AddTransaction from "./components/AddTransaction.vue"
import { useToast } from "vue-toastification"
import { ref, computed, onMounted } from "vue"

const toast = useToast()

const transactions = ref([])

onMounted(() => {
  const saveTransactions = JSON.parse(localStorage.getItem("transactionData"))
  if (saveTransactions) {
    transactions.value = saveTransactions
  }
})

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
  saveTransactionToLocalStorage()
  toast.success("Transaction Added.")

  console.log("transaction", transactions.value)
}

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((tran) => tran.id !== id)
  saveTransactionToLocalStorage()
}

const saveTransactionToLocalStorage = () => {
  localStorage.setItem("transactionData", JSON.stringify(transactions.value))
}

function generateUniqueId() {
  return Math.floor(Math.random() * 10000000)
}
</script>
