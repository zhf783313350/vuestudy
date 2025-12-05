<script setup>
import { ref, reactive } from 'vue'

const formData = reactive({
  username: '',
  email: '',
  age: '',
  gender: '',
  country: 'USA',
  hobbies: [],
  subscribe: false,
  comments: ''
})

const submitted = ref(false)
const submittedData = ref(null)

const countries = ['USA', 'Canada', 'UK', 'Australia', 'Other']
const hobbiesOptions = ['Reading', 'Sports', 'Music', 'Gaming', 'Travel']

const handleSubmit = () => {
  submittedData.value = { ...formData }
  submitted.value = true
  
  // Reset after 3 seconds
  setTimeout(() => {
    submitted.value = false
  }, 3000)
}

const resetForm = () => {
  formData.username = ''
  formData.email = ''
  formData.age = ''
  formData.gender = ''
  formData.country = 'USA'
  formData.hobbies = []
  formData.subscribe = false
  formData.comments = ''
  submitted.value = false
}
</script>

<template>
  <div class="form-example">
    <h2>Form Handling with v-model</h2>
    
    <div class="form-container">
      <form @submit.prevent="handleSubmit">
        <!-- Text Input -->
        <div class="form-group">
          <label>Username:</label>
          <input 
            v-model="formData.username" 
            type="text" 
            placeholder="Enter username"
            required
          />
          <small>Current value: {{ formData.username || 'empty' }}</small>
        </div>

        <!-- Email Input -->
        <div class="form-group">
          <label>Email:</label>
          <input 
            v-model="formData.email" 
            type="email" 
            placeholder="your@email.com"
            required
          />
        </div>

        <!-- Number Input -->
        <div class="form-group">
          <label>Age:</label>
          <input 
            v-model.number="formData.age" 
            type="number" 
            placeholder="Enter age"
            min="1"
            max="120"
          />
        </div>

        <!-- Radio Buttons -->
        <div class="form-group">
          <label>Gender:</label>
          <div class="radio-group">
            <label>
              <input v-model="formData.gender" type="radio" value="male" />
              Male
            </label>
            <label>
              <input v-model="formData.gender" type="radio" value="female" />
              Female
            </label>
            <label>
              <input v-model="formData.gender" type="radio" value="other" />
              Other
            </label>
          </div>
        </div>

        <!-- Select Dropdown -->
        <div class="form-group">
          <label>Country:</label>
          <select v-model="formData.country">
            <option v-for="country in countries" :key="country" :value="country">
              {{ country }}
            </option>
          </select>
        </div>

        <!-- Checkboxes -->
        <div class="form-group">
          <label>Hobbies:</label>
          <div class="checkbox-group">
            <label v-for="hobby in hobbiesOptions" :key="hobby">
              <input v-model="formData.hobbies" type="checkbox" :value="hobby" />
              {{ hobby }}
            </label>
          </div>
          <small>Selected: {{ formData.hobbies.join(', ') || 'none' }}</small>
        </div>

        <!-- Checkbox -->
        <div class="form-group">
          <label class="checkbox-label">
            <input v-model="formData.subscribe" type="checkbox" />
            Subscribe to newsletter
          </label>
        </div>

        <!-- Textarea -->
        <div class="form-group">
          <label>Comments:</label>
          <textarea 
            v-model.trim="formData.comments" 
            placeholder="Enter your comments..."
            rows="4"
          ></textarea>
        </div>

        <!-- Buttons -->
        <div class="button-group">
          <button type="submit" class="submit-btn">Submit</button>
          <button type="button" @click="resetForm" class="reset-btn">Reset</button>
        </div>
      </form>

      <!-- Success Message -->
      <div v-if="submitted" class="success-message">
        <h3>✓ Form Submitted Successfully!</h3>
        <pre>{{ JSON.stringify(submittedData, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>

<style scoped>
.form-example {
  padding: 20px;
  background: #f5f5f5;
  border-radius: 8px;
  margin: 20px 0;
}

.form-container {
  background: white;
  padding: 20px;
  border-radius: 6px;
}

form {
  max-width: 600px;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  color: #333;
}

input[type="text"],
input[type="email"],
input[type="number"],
select,
textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  box-sizing: border-box;
}

small {
  display: block;
  margin-top: 5px;
  color: #666;
  font-size: 12px;
}

.radio-group,
.checkbox-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-top: 8px;
}

.radio-group label,
.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: normal;
  cursor: pointer;
}

.checkbox-label {
  display: flex !important;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.button-group {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

.submit-btn {
  background: #42b883;
  color: white;
}

.submit-btn:hover {
  background: #369970;
}

.reset-btn {
  background: #f0f0f0;
  color: #333;
}

.reset-btn:hover {
  background: #ddd;
}

.success-message {
  margin-top: 20px;
  padding: 15px;
  background: #d4edda;
  border: 1px solid #c3e6cb;
  border-radius: 4px;
  color: #155724;
}

.success-message h3 {
  margin-top: 0;
}

.success-message pre {
  background: white;
  padding: 10px;
  border-radius: 4px;
  overflow-x: auto;
  font-size: 12px;
}
</style>
