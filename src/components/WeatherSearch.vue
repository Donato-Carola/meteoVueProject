<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <div class="d-flex justify-content-between align-items-center">
          <h1 class="text-uppercase ">Weather</h1>

          <form @submit.prevent="searchWeather">
            <div class="input-group">
              <input
                v-model="cityInput"
                type="text"
                class="form-control"
                placeholder="Enter city"
              />
              <button class="btn btn-primary" type="submit">Search</button>
            </div>
          </form>
        </div>
      </div>
      <div class="container">
        <h1 class="current">Previsione attuale</h1>
         {{ current.text }}
         {{ location.name }}
        
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      APIKey: "75a109a67a35426b871104513243003",
      location: [],
      current: [],
      cityInput: "Rome", // Default city
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
          this.location = response.data.location;
          this.current = response.data.current.condition;
        })
        .catch((error) => {
          console.warn(error);
        });
    },
    searchWeather() {
      this.getMeteo(this.cityInput);
    },
  },
  watch: {
    cityInput(newValue, oldValue) {
      this.$emit("city-changed", newValue);
    },
  },
  mounted() {
    // Esempio di chiamata alla funzione getMeteo() con una città di esempio
    this.getMeteo(this.cityInput);
  },
};
</script>

<style lang="scss" scoped>

h1{
  color: aqua;
}

h1.current{
  color: aquamarine;
}
</style>
