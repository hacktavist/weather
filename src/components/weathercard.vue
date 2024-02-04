<template>
  <div class="card-container">
    <div class="card">
      <div v-for="(value, key) in realtimeData" :key="key">
        <h3 v-if="key == 'temperature'">Current Weather</h3>
        <span v-if="key == 'temperature'" class="card-data">
          {{ formatKey(key) }}: {{ value }}</span
        >
      </div>
    </div>
  </div>
  <div class="card-container">
    <div class="card" v-for="(forecast, index) in forecastData" :key="index">
      <h3>{{ formatDate(forecast.time) }}</h3>
      <!-- or use a date formatting function -->
      <div v-for="(value, key) in forecast" :key="key">
        <span class="card-data" v-if="key != 'time' && key != 'name'">
          {{ formatKey(key) }}: {{ value }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    realtimeData: {
      type: Object,
      default: () => ({}),
    },
    forecastData: {
      type: Object,
      default: () => ({}),
    },
  },

  methods: {
    formatKey(key) {
      const words = key.match(/[A-Z][a-z]+|[0-9]+|[a-z]+/g);
      return words
        .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
        .join(" ");
    },
    formatDate(time) {
      time = new Date(time).toLocaleDateString("en-US", {
        month: "2-digit",
        day: "2-digit",
        year: "numeric",
      });
      return time;
    },
  },
};
</script>
<style scoped>
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  padding: 20px;
  justify-content: center;
}

.card {
  display: flex;
  flex-direction: column;
  padding: 20px;
  margin: 10px;
  border: 1px solid #ddd;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  min-width: 200px;
  max-width: 200px;
  flex: 1 1 calc(15% - 40px); /* Adjust flex-basis here, subtracting the total margin and padding */
}

.card-data {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: 5px 0;
  font-size: 14px;
}

.card-data span:first-child {
  font-weight: bold;
}
</style>
