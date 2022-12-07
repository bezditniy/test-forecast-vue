<script>
import Forecast from './components/Forecast.vue'

export default {
  name: "app",
  components: {
    Forecast,
  }
};

</script>

<template>
  <main>
    <div class="wrapper">
      <h1>Weather Forecast</h1>
      <Forecast />
    </div>
  </main>
</template>

<style scoped>
main {
  line-height: 1.5;

}
</style>
