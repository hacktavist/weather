<template>
  <div class="select-container">
    <label>Units</label>
    <select v-model="units" @change="changeUnits">
      <option class="select-options" value="F" selected>&deg;Fahrenheit</option>
      <option class="select-options" value="C">&deg;Celsius</option>
    </select>
  </div>
</template>
<script>
export default {
  data() {
    return {
      units: "F",
      unitType: null,
    };
  },
  methods: {
    changeUnits() {
      let unitType = this.units === "F" ? "imperial" : "metric";
      this.$emit("unit-changed", unitType);
    },
  },
};
</script>
<style scoped>
.select-container {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: 0 auto;
  padding-bottom: 50px;
}
.select-options {
  text-align: center;
}
</style>
