<template>
  <div id="app">
    <h1>Vue.js + Express Template</h1>
    <button @click="fetchMessage">Get Message from Backend</button>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: '',
    };
  },
  methods: {
    async fetchMessage() {
      try {
        const response = await fetch('http://localhost:3000');
        const data = await response.json();
        this.message = data.message;
      } catch (error) {
        console.error('Error fetching message:', error);
      }
    },
  },
};
</script>
