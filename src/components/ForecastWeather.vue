<template>
    <div class="container pt-5">
      <h2>Previsioni meteo a lungo termine per {{ city }}</h2>
      
        <div v-for="day in forecast" :key="day.date">
          <h3>{{ day.date }}</h3>
          <p>Massima temperatura: {{ day.day.maxtemp_c }} °C</p>
          <p>Minima temperatura: {{  day.day.mintemp_c }} °C</p>
          <!-- Aggiungi altri dettagli delle previsioni se necessario -->
        </div>
      
      
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    props: {
      city: String
    },
    data() {
      return {
        APIKey: "75a109a67a35426b871104513243003",
        forecast: [],
      };
    },
    methods: {
      getForecast(city) {
        axios
          .get(
            `http://api.weatherapi.com/v1/forecast.json?q=${city}&key=${this.APIKey}&days=14`
          )
          .then((response) => {
            console.log('Forecast Data:', response.data);
            // Assegnamento della risposta all'oggetto previsioni a lungo termine
            this.forecast = response.data.forecast.forecastday;
          })
          .catch((error) => {
            console.error('Errore nella richiesta API forecast:', error);
            // Puoi anche visualizzare un messaggio di errore per l'utente
          });
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