<template>
  <div class="slider-container">
    <input type="range" :min="minValue" :max="maxValue" v-model="startValue" @input="updateRange">
    <input type="range" :min="minValue" :max="maxValue" v-model="endValue" @input="updateRange">
  </div>
</template>

<script>
export default {
  data() {
    return {
      startValue: 1,
      endValue: 151,
      minValue: 1,
      maxValue: 151
    };
  },
  methods: {
    updateRange() {
      if (this.startValue > this.endValue) {
        this.endValue = this.startValue;
      }
      this.$emit('update-range', { start: this.startValue, end: this.endValue });
    }
  }
};
</script>

<style scoped>
.slider-container {
  padding: 10px;
  text-align: center;
}

input[type="range"] {
  width: 45%;
  margin: 0 2.5%;
}
</style>
