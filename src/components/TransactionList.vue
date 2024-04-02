<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      v-bind:class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button @click="deleteThisTransaction(transaction.id)" class="delete-btn">x</button>
    </li>
    <!-- <li class="plus">Salary <span>$400</span><button class="delete-btn">x</button></li> -->
  </ul>
</template>
<script setup>
import { defineProps } from 'vue'
const props = defineProps({
  transactions: {
    type: Array,
    required: true
  }
})
const emit = defineEmits(['removeTransaction'])
const deleteThisTransaction = (id) => {
  emit('removeTransaction', id)
}
</script>
