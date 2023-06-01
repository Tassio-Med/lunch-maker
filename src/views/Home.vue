<template>
  <Banner/>
  <div class="main-container">
    <h1>HOME</h1>
    <LunchForm />
  </div>
</template>

<script>

import Banner from '../components/Banner.vue';
import LunchForm from '../components/LunchForm.vue';

  export default {
    name: "Home",
    components: {
      Banner,
      LunchForm
    }
  }
</script>
<style scoped>
  
</style>
