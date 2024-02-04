<template>
  <div class="search-container">
    <label>{{ label }}</label>
    <input type="text" v-model="city" name="city" @keyup.enter="emit" />
  </div>
</template>

<script>
export default {
  props: {
    label: String,
  },
  data() {
    return {
      city: "",
    };
  },
  methods: {
    emit() {
      this.$emit("search", this.city);
    },
  },
};
</script>
<style scoped>
.search-container {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: 0 auto;
  padding-bottom: 50px;
}
</style>
