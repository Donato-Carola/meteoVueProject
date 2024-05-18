<template>
  <div class="container">
    <div
      class="d-md-flex d-sm-inline-block justify-content-between align-items-center"
    >
      <h1 class="text-uppercase my_title">Weather</h1>

      <form @submit.prevent="searchWeather">
        <div class="input-group">
          <input
            v-model="cityInput"
            type="text"
            class="form-control"
            placeholder="Enter city"
          />
          <button class="btn " type="submit">Search</button>
        </div>
      </form>
    </div>

    <div class="container pt-3">
      <div class="text-center">
        <h1 class="current">Previsione attuale</h1>

        <div class="card my_back border-0 ">
          <div>
            <img
              v-if="currentWeatherIcon"
              class="card-img-top container w-25"
              :src="currentWeatherIcon"
              alt="Weather Icon"
            />
          </div>
          <h1>{{ location.name }}</h1>
          {{ current.temp_c }}
          <!-- <h3>{{ data.current.temp_c}} °C</h3> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import weatherIconsData from "../data/weatherIcons.json";

export default {
  data() {
    return {
      APIKey: "75a109a67a35426b871104513243003",
      // data:[],
      location: [],
      current: [],
      cityInput: "Rome", // Default city
      weatherIcons: weatherIconsData,
      currentWeatherIcon: null,
    };
  },
  methods: {
    getMeteo(city) {
      axios
        .get(
          `http://api.weatherapi.com/v1/current.json?q=${city}&key=${this.APIKey}`
        )
        .then((response) => {
          console.log("Data", response.data);
          console.log("Condizioni correnti", response.data.current.condition);
          console.log("Location", response.data.location);
          // Assegnamento della risposta all'oggetto previsioni
          this.data = response.data;
          this.location = response.data.location;
          this.current = response.data.current;
          if (response.data.current.condition.text in this.weatherIcons) {
            this.currentWeatherIcon =
              this.weatherIcons[response.data.current.condition.text];
          } else {
            // Se non c'è un'icona corrispondente, puoi gestire un'icona di default o un messaggio di errore
            this.currentWeatherIcon = "https://www.example.com/default.jpg";
          }
          this.$emit("city-changed", this.cityInput);
        })
        .catch((error) => {
          console.warn(error);
        });
    },
    searchWeather() {
      this.getMeteo(this.cityInput);
    },
  },

  computed: {
    currentWeatherIcon() {
      if (this.current.condition && this.current.condition.text) {
        // Cerca nel file JSON l'icona corrispondente al testo delle condizioni meteorologiche
        for (const icon of this.weatherIcons) {
          if (icon.day === this.current.condition.text) {
            // Costruisci l'URL dell'immagine utilizzando il codice dell'icona
            return `https://cdn.weatherapi.com/weather/64x64/day/${icon.icon}.png`;
          } else if (icon.night === this.current.condition.text) {
            return `https://cdn.weatherapi.com/weather/64x64/night/${icon.icon}.png`;
          }
        }
      }
      // Se non c'è un'icona corrispondente nel file JSON, puoi gestire un'icona di default
      return "https://www.example.com/default.jpg";
    },
  },

  mounted() {
    // Esempio di chiamata alla funzione getMeteo() con una città di esempio
    this.getMeteo(this.cityInput);
  },
};
</script>

<!-- stile -->

<style lang="scss" scoped>


h1.current,
.my_title{
  color: rgb(52, 209, 157);
}

button{
  background-color: rgb(103, 224, 178);
}


</style>
