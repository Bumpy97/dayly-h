<template>
    <div class="loading-container" v-if="loading">
      <div class="stars"></div>
      <img src="../assets/m.png" alt="Loading Icon" class="loading-icon">
      <div class="loading-text">{{ progress }}%</div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        loading: true,
        progress: 0
      };
    },
    methods: {
      startLoading() {
        let interval = setInterval(() => {
          if (this.progress < 100) {
            this.progress += 1;
          } else {
            clearInterval(interval);
            this.loading = false;
            this.$emit('loading-complete');
          }
        }, 35); // скорость загрузки
      }
    },
    mounted() {
      this.startLoading();
    }
  };
  </script>
  
  <style>
  
  </style>
  