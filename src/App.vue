<template>
  <div>
    <!-- Componente per la ricerca delle previsioni attuali -->
    <WeatherSearch @city-changed="updateCity" />

    <!-- Componente per le previsioni meteo a lungo termine -->
    <ForecastWeather :city="cityInput" v-if="cityInput" />
  </div>
</template>

<script>
import WeatherSearch from './components/WeatherSearch.vue';
import ForecastWeather from './components/ForecastWeather.vue';

export default {
  components: {
    WeatherSearch,
    ForecastWeather,
  },
  data() {
    return {
      cityInput: "Rome", // Default city
    };
  },
  methods: {
    updateCity(newCity) {
      this.cityInput = newCity;
    },
  },
};
</script>

<style lang="scss">
@use "./style/general.scss" as *;
</style>
