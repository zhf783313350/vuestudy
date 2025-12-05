<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

// Computed property - automatically updates when dependencies change
const fullName = computed(() => {
  return `${firstName.value} ${lastName.value}`
})

const items = ref([
  { name: 'Apple', price: 1.5, quantity: 3 },
  { name: 'Banana', price: 0.8, quantity: 5 },
  { name: 'Orange', price: 1.2, quantity: 2 }
])

// Computed property for total price
const totalPrice = computed(() => {
  return items.value.reduce((sum, item) => sum + (item.price * item.quantity), 0).toFixed(2)
})
</script>

<template>
  <div class="computed-example">
    <h2>Computed Properties Example</h2>
    
    <div class="section">
      <h3>Name Builder</h3>
      <div class="inputs">
        <input v-model="firstName" placeholder="First name" />
        <input v-model="lastName" placeholder="Last name" />
      </div>
      <p class="result">Full Name: <strong>{{ fullName }}</strong></p>
    </div>

    <div class="section">
      <h3>Shopping Cart Total</h3>
      <table>
        <thead>
          <tr>
            <th>Item</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>Subtotal</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" :key="item.name">
            <td>{{ item.name }}</td>
            <td>${{ item.price }}</td>
            <td>{{ item.quantity }}</td>
            <td>${{ (item.price * item.quantity).toFixed(2) }}</td>
          </tr>
        </tbody>
      </table>
      <p class="result">Total: <strong>${{ totalPrice }}</strong></p>
    </div>
  </div>
</template>

<style scoped>
.computed-example {
  padding: 20px;
  background: #f5f5f5;
  border-radius: 8px;
  margin: 20px 0;
}

.section {
  margin: 20px 0;
  padding: 15px;
  background: white;
  border-radius: 6px;
}

.inputs {
  display: flex;
  gap: 10px;
  margin: 10px 0;
}

input {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.result {
  margin-top: 15px;
  font-size: 16px;
  color: #333;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin: 10px 0;
}

th, td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background: #f0f0f0;
  font-weight: 600;
}
</style>
