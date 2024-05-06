<template>
  <div class="weather">
    <div class="container">
      <div class="card weather-form">
        <input
          type=" text "
          class="weather-form__input"
          v-model="searchQuery"
          @keyup.enter="weatherSearch"
          placeholder="Enter city"
        />
        <button class="weather-form__btn" @click="weatherSearch">Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading ...</div>

      <div
        class="weather-info"
        v-show="!error && location && temperature !== 0 && description"
      >
        <div class="card" v-if="error">Error</div>

        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }} C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>

  </div>
</template>
<script>
export default {
  data() {
    return {
      location: " ",
      temperature: 0,
      description: " ",
      loading: false,
      error: false,
      searchQuery: " ",
    };
  },
  computed: {},
  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(
        `http://api.weatherapi.com/v1/current.json?key=e3ba1403e9c24e1aa9e104419240605&q=${this.searchQuery}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
        .catch((error) => {
          this.loading = false;
          this.error = true;
          console.error(error);
        });
    },
    resetSearchQuery() {
      this.searchQuery = " ";
    },
  },
};
</script>
