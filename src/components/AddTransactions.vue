<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" v-model="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input type="text" v-model="amount" id="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
<script setup>
import { useToast } from 'vue-toastification'
import { ref } from 'vue'
const emit = defineEmits(['submitForm'])
const toast = useToast()
const text = ref('')
const amount = ref('')

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Please fill both fields.')
    return
  }
  if (isNaN(amount.value)) {
    toast.error('Please enter a valid amount.')
    return
  }
  const formData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }
  emit('submitForm', formData)
  amount.value = ''
  text.value = ''
}
</script>
