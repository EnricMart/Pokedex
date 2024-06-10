<template>
  <div class="slider-container">
    <input type="range" :min="minValue" :max="midValue" v-model="startValue" @input="updateStartValue">
    <input type="range" :min="midValuePlusOne" :max="maxValue" v-model="endValue" @input="updateEndValue">
  </div>
</template>

<script>
export default {
  data() {
    return {
      startValue: 1,
      endValue: 151,
      minValue: 1,
      midValue: 75,
      midValuePlusOne: 76,
      maxValue: 151
    };
  },
  methods: {
    updateStartValue() {
      if (this.startValue > this.midValue) {
        this.startValue = this.midValue;
      }
      this.$emit('update-range', { start: this.startValue, end: this.endValue });
    },
    updateEndValue() {
      if (this.endValue < this.midValuePlusOne) {
        this.endValue = this.midValuePlusOne;
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
