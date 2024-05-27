<template>
  <div>
    <button @click="toggleShowTienda">Mostrar Tienda</button>
    <ShowTienda v-if="showTienda" @update-inventory="updateInventory"></ShowTienda>

    <button @click="toggleShowInventario">Mostrar Inventario</button>
    <div v-if="showInventario" class="modal">
      <div class="modal-content">
        <span @click="toggleShowInventario" class="close">&times;</span>
        <ShowInventario :items="inventoryItems"></ShowInventario>
      </div>
    </div>

    <PokedexDisplay></PokedexDisplay>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import PokedexDisplay from './components/PokedexDisplay.vue';
import ShowTienda from './components/ShowTienda.vue';
import ShowInventario from './components/ShowInventario.vue';

const showTienda = ref(false);
const showInventario = ref(false);
const inventoryItems = ref([]);

const toggleShowTienda = () => {
  showTienda.value = !showTienda.value;
};

const toggleShowInventario = () => {
  showInventario.value = !showInventario.value;
};

const updateInventory = (items) => {
  inventoryItems.value = items;
};
</script>

<style scoped>
/* Estilos del modal */
.modal {
  display: block; /* Mostrar el modal */
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0,0,0,0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
