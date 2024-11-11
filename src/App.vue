<template>
  <v-app class="purple lighten-5">
    <v-container class="mt-6">
      <v-row justify="center">
        <v-col cols="12" md="6">
          <v-card class="elevation-12 purple darken-3">
            <v-card-title class="d-flex justify-center">
              <h2 class="text-center text-h4 text-Black">Weather App</h2>
            </v-card-title>
            <v-card-text class="pa-4">
              <v-text-field
                v-model="city"
                label="Enter city"
                outlined
                color="secondary"
                @keyup.enter="fetchWeather"
                class="mb-3 white--text"
              ></v-text-field>
              <v-btn
                color="secondary"
                dark
                class="mb-4 white--text"
                @click="fetchWeather"
              >
                Get Weather
              </v-btn>
              <v-alert v-if="error" type="error" outlined class="mt-2">
                {{ error }}
              </v-alert>
              <v-card v-if="weather" class="mt-4 elevation-2 weather-card">
                <v-card-title class="text-h5">
                  <span class="text-secondary font-weight-bold">{{
                    weather.name
                  }}</span>
                </v-card-title>
                <v-card-subtitle class="text-body-2 text-uppercase">
                  {{ weather.weather[0].description }}
                </v-card-subtitle>
                <v-card-text>
                  <v-row>
                    <v-col>
                      <p class="text-h6 white--text">
                        Temperature: <strong>{{ weather.main.temp }} °C</strong>
                      </p>
                      <p class="text-h6 white--text">
                        Humidity: <strong>{{ weather.main.humidity }}%</strong>
                      </p>
                      <p class="text-h6 white--text">
                        Wind Speed:
                        <strong>{{ weather.wind.speed }} m/s</strong>
                      </p>
                    </v-col>
                  </v-row>
                </v-card-text>
              </v-card>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      city: "",
      weather: null,
      error: "",
    };
  },
  methods: {
    async fetchWeather() {
      if (this.city.trim() === "") {
        this.error = "Please enter a city name.";
        return;
      }
      try {
        const apiKey = "abb7c61f8807459a703d8f3ec4160e8c"; // Replace with your OpenWeatherMap API key
        const response = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&ap
pid=${apiKey}`
        );
        const data = await response.json();
        if (data.cod === 200) {
          this.weather = data;
          this.error = "";
        } else {
          this.error = "City not found.";
          this.weather = null;
        }
      } catch (error) {
        this.error = "Unable to fetch weather data.";
        this.weather = null;
      }
    },
  },
};
</script>
<style>
body {
  font-family: "Roboto", sans-serif;
}

.weather-card {
  background: linear-gradient(135deg, #673ab7, #311b92);
  color: #ffffff;
}

.text-h4 {
  font-size: 1.6rem;
}

.text-h6 {
  font-size: 1.2rem;
  font-weight: 400;
}
</style>
