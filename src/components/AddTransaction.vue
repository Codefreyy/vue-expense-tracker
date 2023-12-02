<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
>

<script setup>
import { ref, defineEmits } from "vue"
import { useToast } from "vue-toastification"

const amount = ref("")
const text = ref("")
const toast = useToast()
const emit = defineEmits(["transactionDataSubmitted"])

function onSubmit() {
  if (!text.value || !amount.value) {
    toast.error("Both fields are required.")
    return
  }

  if (Number(amount.value).toString() == "NaN") {
    toast.error("Amount should be numbers")
    return
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  }

  emit("transactionDataSubmitted", transactionData)
  text.value = ""
  amount.value = ""
}
</script>
