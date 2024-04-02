<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
  </div>
  <IncomeExpense :income="+income" :expenses="+expenses" />
  <TransactionList :transactions="transactions" @removeTransaction="removeTransactionFromList" />
  <AddTransaction @submitForm="submittedFormData" />
</template>
<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpense from './components/IncomeExpense.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransactions.vue'
import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'
const toast = useToast()
const transactions = ref([])
const total = computed(() => {
  return transactions.value
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})
onMounted(() => {
  const saveTransactions = JSON.parse(localStorage.getItem('transactions'))
  if (saveTransactions) {
    transactions.value = saveTransactions
  }
})
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

const submittedFormData = (submitForm) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: submitForm.text,
    amount: submitForm.amount
  })
  saveTransactionToLocalStorage()
  toast.success('Transaction added successfully.')
}
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}
const removeTransactionFromList = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id != id)
  saveTransactionToLocalStorage()
  toast.success('Transaction removed successfully.')
}
const saveTransactionToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>
