<template>
    <div class="compra-boton">
      <button @click="decrement">-</button>
      <span>{{ quantity }}</span>
      <button @click="increment">+</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      quantity: {
        type: Number,
        required: true
      }
    },
    methods: {
      increment() {
        this.$emit('update-quantity', this.quantity + 1);
      },
      decrement() {
        if (this.quantity > 0) {
          this.$emit('update-quantity', this.quantity - 1);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .compra-boton {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  button {
    margin: 0 5px;
    padding: 5px 10px;
  }
  span {
    width: 20px;
    text-align: center;
  }
  </style>
  