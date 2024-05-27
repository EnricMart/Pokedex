<template>
    <div class="item-card">
      <img :src="item.imageUrl" :alt="item.name" class="img-fluid">
      <h3>{{ item.name }}</h3>
      <p>Cantidad: {{ item.quantity }}</p>
      <CompraBoton :quantity="item.quantity" @update-quantity="updateQuantity"></CompraBoton>
    </div>
  </template>
  
  <script>
  import CompraBoton from './CompraBoton.vue';
  
  export default {
    components: {
      CompraBoton
    },
    props: {
      item: {
        type: Object,
        required: true
      }
    },
    methods: {
      updateQuantity(newQuantity) {
        this.$emit('update-quantity', this.item.id, newQuantity);
      }
    }
  };
  </script>
  
  <style scoped>
  .item-card {
    border: 1px solid #ccc;
    padding: 10px;
    text-align: center;
  }
  </style>
  