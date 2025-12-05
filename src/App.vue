<script setup>
import { ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import ComputedExample from './components/ComputedExample.vue'
import ListExample from './components/ListExample.vue'
import FormExample from './components/FormExample.vue'

const activeTab = ref('basics')
</script>

<template>
  <div class="app-container">
    <!-- Header -->
    <header class="app-header">
      <div class="logos">
        <a href="https://vite.dev" target="_blank">
          <img src="/vite.svg" class="logo" alt="Vite logo" />
        </a>
        <a href="https://vuejs.org/" target="_blank">
          <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
        </a>
      </div>
      <h1>Vue 3 Study Project</h1>
      <p class="subtitle">Learn Vue 3 with Interactive Examples</p>
    </header>

    <!-- Navigation Tabs -->
    <nav class="tabs">
      <button 
        @click="activeTab = 'basics'" 
        :class="{ active: activeTab === 'basics' }"
      >
        Basics
      </button>
      <button 
        @click="activeTab = 'computed'" 
        :class="{ active: activeTab === 'computed' }"
      >
        Computed Properties
      </button>
      <button 
        @click="activeTab = 'lists'" 
        :class="{ active: activeTab === 'lists' }"
      >
        Lists & Conditionals
      </button>
      <button 
        @click="activeTab = 'forms'" 
        :class="{ active: activeTab === 'forms' }"
      >
        Forms
      </button>
    </nav>

    <!-- Content -->
    <main class="content">
      <HelloWorld v-show="activeTab === 'basics'" msg="Vue 3 + Vite" />
      <ComputedExample v-show="activeTab === 'computed'" />
      <ListExample v-show="activeTab === 'lists'" />
      <FormExample v-show="activeTab === 'forms'" />
    </main>

    <!-- Footer -->
    <footer class="app-footer">
      <p>Built with Vue 3 and Vite | Learn more at <a href="https://vuejs.org" target="_blank">vuejs.org</a></p>
    </footer>
  </div>
</template>

<style scoped>
.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.app-header {
  text-align: center;
  margin-bottom: 30px;
}

.logos {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 20px;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

h1 {
  font-size: 2.5em;
  margin: 0;
  color: #2c3e50;
}

.subtitle {
  font-size: 1.2em;
  color: #666;
  margin-top: 10px;
}

.tabs {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
  border-bottom: 2px solid #e0e0e0;
  padding-bottom: 10px;
  flex-wrap: wrap;
}

.tabs button {
  padding: 10px 20px;
  background: transparent;
  border: none;
  border-bottom: 3px solid transparent;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  color: #666;
  transition: all 0.3s;
}

.tabs button:hover {
  color: #42b883;
}

.tabs button.active {
  color: #42b883;
  border-bottom-color: #42b883;
}

.content {
  min-height: 400px;
}

.app-footer {
  margin-top: 50px;
  padding: 20px;
  text-align: center;
  color: #666;
  border-top: 1px solid #e0e0e0;
}

.app-footer a {
  color: #42b883;
  text-decoration: none;
}

.app-footer a:hover {
  text-decoration: underline;
}

@media (max-width: 768px) {
  h1 {
    font-size: 2em;
  }
  
  .tabs {
    justify-content: center;
  }
  
  .tabs button {
    padding: 8px 16px;
    font-size: 14px;
  }
}
</style>
