<template>
  <app-weather />
     <!-- <font-awesome-icon :icon="['fas', 'location-crosshairs']" /> -->
</template>
<script>
import Weather from './components/OpenWeather.vue'
export default {
  components: {
    'app-weather' : Weather
  },
}
</script>
