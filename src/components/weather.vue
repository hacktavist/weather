<template>
  <Searchbar @search="handleSearch" :label="searchLabel" />
  <UnitSelect @unit-changed="handleUnitChange" />
  <WeatherCard
    v-if="realtimeData || forecastData"
    :realtimeData="realtimeData"
    :forecastData="forecastData"
  />
</template>

<script>
import Searchbar from "@/components/searchbar.vue";
import WeatherCard from "@/components/weathercard.vue";
import UnitSelect from "@/components/unitSelect.vue";
import qs from "qs";

const urlEndpoint = "https://api.tomorrow.io/v4/";
const key = import.meta.env.VITE_API_KEY;
const weatherRoute = "weather/";
const realtime = "realtime?";
const forecast = "forecast?";
let unit = "";

export default {
  components: {
    Searchbar,
    WeatherCard,
    UnitSelect,
  },
  data() {
    return {
      realtimeData: null,
      forecastData: null,
      units: "imperial",
      searchLabel: "City, State, or Zip/Postal",
    };
  },
  mounted() {
    this.handleUnitChange(this.units);
  },
  methods: {
    handleSearch(query) {
      this.getWeatherRealtime(query);
      this.getWeatherForecast(query);
    },
    handleUnitChange(type) {
      unit = type;
    },
    async getWeatherRealtime(query) {
      const options = {
        method: "GET",
        headers: { accept: "application/json", "Accept-Encoding": "gzip" },
      };
      const route = urlEndpoint + weatherRoute + realtime;
      const urlParams = qs.stringify({
        apikey: key,
        location: query,
        units: unit,
      });
      fetch(`${route}${urlParams}`, options)
        .then((response) => response.json())
        .then((response) => {
          const {
            data: {
              time,
              values: {
                cloudCover,
                humidity,
                precipitationProbability,
                temperature,
                windGust,
                windSpeed,
              },
            },
            location: { name },
          } = response;
          this.realtimeData = {
            time,
            cloudCover,
            humidity,
            precipitationProbability,
            temperature,
            windGust,
            windSpeed,
            name,
          };
        })
        .catch((err) => console.error(err));
    },
    async getWeatherForecast(query) {
      const options = {
        method: "GET",
        headers: { accept: "application/json", "Accept-Encoding": "gzip" },
      };
      const route = urlEndpoint + weatherRoute + forecast;
      const urlParams = qs.stringify(
        {
          apikey: key,
          location: query,
          units: unit,
          timesteps: ["1d"],
        },
        { arrayFormat: "comma" }
      );
      fetch(`${route}${urlParams}`, options)
        .then((response) => response.json())
        .then((response) => {
          const {
            timelines: { daily },
            location: { name },
          } = response;

          this.forecastData = daily.map((day) => {
            const {
              time,
              values: { temperatureAvg, temperatureMax, temperatureMin },
            } = day;
            return {
              time,
              temperatureAvg,
              temperatureMax,
              temperatureMin,
              name,
            };
          });
        })
        .catch((err) => console.error(err));
    },
  },
};
</script>
