<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input type="text" class="weather-form__input" placeholder="Enter city" v-model="searchQuery"
               @keyup.enter="weatherSearch">
        <button class="weather-form__btn" @click="weatherSearch">Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>
      <div class="error card" v-if="error">Error</div>
      <div class="weather-info" v-show="!error && location && temperature && description">

        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }}°C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>

    <div class="weather-bg">
      <div>
        <img class="weather-bg__img bg" src="./assets/bg.png" alt="App Background">
        <img class="weather-bg__img overcast" src="./assets/overcast.png" alt="App Background">
        <img class="weather-bg__img partly-cloudy" src="./assets/cloudy.png" alt="Partly Cloudy">
        <img class="weather-bg__img sunny" src="./assets/sunny.png" alt="Sunny">
        <img class="weather-bg__img rainy" src="./assets/rainy.png" alt="Rainy">
        <img class="weather-bg__img clear" src="./assets/clear.png" alt="Clear">
      </div>
    </div>

  </div>
</template>

<script>

export default {
  data() {
    return {
      location: "",
      temperature: 0,
      description: "",
      loading: false,
      error: false,
      searchQuery: "",

    }
  },
  computed: {
    weatherClass() {
      if (this.description.toLowerCase().includes("rain")) {
        return "rainy";
      } else if (this.description.toLowerCase().includes("overcast")) {
        return "overcast";
      } else if (this.description.toLowerCase().includes("partly cloudy")) {
        return "partly-cloudy";
      } else if (this.description.toLowerCase().includes("sunny")) {
        return "sunny";
      } else if (this.description.toLowerCase().includes("clear")) {
        return "clear";
      } else {
        return "";
      }
    }
  },
  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(`https://api.weatherapi.com/v1/current.json?key=69ed7b91ab4b4d4f9f282327242604&q=${this.searchQuery}`)
          .then(response => response.json())
          .then(data => {
            this.loading = false;
            this.location = data.location.name;
            this.temperature = data.current.temp_c;
            this.description = data.current.condition.text;
            this.resetSearchQueary()
          })
          .catch(error => {
            this.loading = false;
            this.error = true;
            console.error(error);
          });
    },
    resetSearchQueary() {
      this.searchQuery = "";
    },

  }

}
</script>

<!--7d0377b3d0234b4da0b114513240110-->