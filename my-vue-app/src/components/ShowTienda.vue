<template>
  <div class="container">
    <div class="row">
      <div v-for="item in items" :key="item.id" class="col-md-3 mb-4">
        <Item :item="item" @update-quantity="updateItemQuantity"></Item>
      </div>
    </div>
  </div>
</template>

<script>
import Item from './Item.vue';

export default {
  components: {
    Item
  },
  data() {
    return {
      items: []
    };
  },
  methods: {
    updateItemQuantity(id, quantity) {
      const item = this.items.find(item => item.id === id);
      if (item) {
        item.quantity = quantity;
        this.saveInventory();
        this.$emit('update-inventory', this.items);
      }
    },
    fetchItemData() {
      fetch('https://pokeapi.co/api/v2/item?limit=20')
        .then(response => response.json())
        .then(data => {
          const itemPromises = data.results.map(item => {
            return fetch(item.url)
              .then(response => response.json())
              .then(itemData => ({
                id: itemData.id,
                name: itemData.name,
                imageUrl: itemData.sprites.default,
                quantity: 0
              }));
          });
          Promise.all(itemPromises)
            .then(items => {
              this.items = items;
              this.loadInventory();
            })
            .catch(err => console.error("Error fetching item data: ", err));
        })
        .catch(err => console.error("Error fetching API: ", err));
    },
    saveInventory() {
      localStorage.setItem('inventory', JSON.stringify(this.items));
    },
    loadInventory() {
      const inventory = localStorage.getItem('inventory');
      if (inventory) {
        this.items = JSON.parse(inventory);
      }
    }
  },
  created() {
    this.fetchItemData();
  }
};
</script>

<style scoped>
.container {
  padding: 20px;
}
</style>
