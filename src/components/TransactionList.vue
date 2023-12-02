<template>
  <h3>History</h3>
  <ul id="list" class="list" v-if="transactions.length">
    <li
      v-for="transaction in transactions"
      :key="transaction.key"
      :class="transaction.amount > 0 ? 'plus' : 'minus'"
    >
      {{ transaction.text }} <span>{{ transaction.amount }}</span
      >><button class="delete-btn" @click="deleteTransaction(transaction.id)">
        x
      </button>
    </li>
  </ul>
  <p v-else>No transactions yet.</p>
</template>
>

<script setup>
import { defineProps, defineEmits } from "vue"

const emit = defineEmits(["transactionDeleted"])

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
    default: () => [],
  },
})

const deleteTransaction = (id) => {
  emit("transactionDeleted", id)
}
</script>
