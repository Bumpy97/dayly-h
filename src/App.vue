<script setup>
import { RouterView } from 'vue-router'
import bridge from '@vkontakte/vk-bridge';
bridge.send("VKWebAppInit");
bridge.send('VKWebAppShowBannerAd', {
  banner_location: 'bottom'
  })
 .then((data) => { 
    if (data.result) {
      // Баннерная реклама отобразилась
    }
  })
  .catch((error) => {
    // Ошибка
    console.log(error);
  });
</script>


<template>
  <div>

  </div>
  <RouterView />
</template>
