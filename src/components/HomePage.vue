<template>
  <div>
    
    <div class="home" v-if="!showLoading">
      <img src="../assets/m.png" alt="">
      <h1>Предсказания на день</h1>
      <h2>Сегодня {{ currentDate }}</h2>
      <button @click="startLoading" class="b1">Узнать предсказание</button>
    </div>
    <LoadingAnimation v-if="showLoading" @loading-complete="navigateToDetails" />
  </div>
</template>

<script>

import LoadingAnimation from './LoadingAnimation.vue';



export default {
  name: 'HomePage',
  components: {
    LoadingAnimation
  },
  data() {
    return {
      currentDate: this.getFormattedDate(),
      showLoading: false
    };
  },
  methods: {
    getFormattedDate() {
      const options = { month: 'long', day: 'numeric' };
      return new Intl.DateTimeFormat('ru-RU', options).format(new Date());
    },
    startLoading() {
      this.showLoading = true;
    },
    navigateToDetails() {
      this.$router.push('/daily');
    }
  }
};
</script>
