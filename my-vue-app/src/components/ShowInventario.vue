<template>
  <div class="container">
    <div class="row">
      <div v-for="item in filteredItems" :key="item.id" class="col-md-3 mb-4">
        <div class="item-card">
          <img :src="item.imageUrl" :alt="item.name" class="img-fluid">
          <h3>{{ item.name }}</h3>
          <p>Cantidad: {{ item.quantity }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true
    }
  },
  computed: {
    filteredItems() {
      return this.items.filter(item => item.quantity > 0);
    }
  }
};
</script>

<style scoped>
.container {
  padding: 20px;
}

.item-card {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
}
</style>
