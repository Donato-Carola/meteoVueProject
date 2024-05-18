<template>
  <div class="container pt-5">
    <!-- <h2>Previsioni meteo nei prossimi giorni</h2>
    <h1>{{ city }}</h1> -->
    <div class="container pt-3">
      <div class="row justify-content-center align-items-center">
        <div class="col-sm-12 col-md-6 col-lg-4 mb-4 align-self-center  " v-for="day in forecast" :key="day.date">
          <h3>{{ getDayOfWeek(day.date) }}</h3>
          <img :src="getWeatherIcon(day.day.condition.text)" :alt="day.day.condition.text" class="weather-icon">
          <p>{{ day.day.condition.text }}</p>
          <p> Max: {{ day.day.maxtemp_c }} °C</p>
          <p> Min: {{ day.day.mintemp_c }} °C</p>
          <!-- Aggiungi altri dettagli delle previsioni se necessario -->
        </div>
      </div>
    </div>
  </div>
  
</template>

<script>
import axios from "axios";
import weatherIconsData from "../data/weatherIcons.json";


export default {
  props: {
    city: String,
  },
  data() {
    return {
      APIKey: "75a109a67a35426b871104513243003",
      forecast: [],
      weatherIcons: weatherIconsData,
    };
  },
  methods: {
    getDayOfWeek(date) {
      const daysOfWeek = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      const dayIndex = new Date(date).getDay();
      return daysOfWeek[dayIndex];
    },
    getForecast(city) {
      axios
        .get(
          `http://api.weatherapi.com/v1/forecast.json?q=${city}&key=${this.APIKey}&days=3`
        )
        .then((response) => {
          console.log("Forecast Data:", response.data.forecast.forecastday);
          // Assegnamento della risposta all'oggetto previsioni a lungo termine
          this.forecast = response.data.forecast.forecastday;
        })
        .catch((error) => {
          console.error("Errore nella richiesta API forecast:", error);
          // Puoi anche visualizzare un messaggio di errore per l'utente
        });
    },

    getWeatherIcon(conditionText) {
      const weatherIcon = this.weatherIcons.find(icon => {
        return icon.day === conditionText || icon.night === conditionText;
      });
      if (weatherIcon) {
        // Se l'icona è trovata, costruisci l'URL e restituiscilo
        return `https://cdn.weatherapi.com/weather/64x64/day/${weatherIcon.icon}.png`;
      } else {
        // Se non è trovata un'icona corrispondente, restituisci l'URL di un'icona di default
        return "https://www.example.com/default.png";
      }
    },

  },
  watch: {
    city(newCity, oldCity) {
      // Aggiornamento delle previsioni quando la città cambia
      this.getForecast(newCity);
    },
  },
  mounted() {
    // Esempio di chiamata alla funzione getForecast() con la città corrente
    this.getForecast(this.city);
  },
};
</script>

<style>
/* Stili se necessario */
</style>
