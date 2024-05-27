<template>
  <div>
    <ShowSlider @update-range="updateVisibleRange"></ShowSlider>
    <div class="container">
      <div class="row">
        <div class="col-md-12 mb-4">
          <button @click="toggleFavorites">Mostrar Favoritos</button>
          <select @change="filterByType($event.target.value)">
            <option value="">Todos los Tipos</option>
            <option v-for="type in allTypes" :key="type" :value="type">{{ type }}</option>
          </select>
        </div>
      </div>
      <div class="row pokemon-grid">
        <div v-for="pokemon in visiblePokemons" :key="pokemon.id" class="col-md-3 mb-4">
          <div class="pokemon-card">
            <img :src="pokemon.imageUrl" :alt="pokemon.name" class="img-fluid">
            <h3>{{ pokemon.name }}</h3>
            <button @click="toggleFavorite(pokemon)">
              {{ pokemon.favorite ? 'Quitar de Favoritos' : 'Añadir a Favoritos' }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ShowSlider from './ShowSlider.vue';

export default {
  components: {
    ShowSlider
  },
  data() {
    return {
      pokemons: [],
      favorites: [],
      allTypes: [],
      showFavorites: false,
      selectedType: '',
      visibleRange: { start: 1, end: 151 }
    };
  },
  computed: {
    filteredPokemons() {
      if (this.showFavorites) {
        return this.pokemons.filter(pokemon => this.favorites.includes(pokemon.id));
      } else if (this.selectedType) {
        return this.pokemons.filter(pokemon => pokemon.types.includes(this.selectedType));
      } else {
        return this.pokemons;
      }
    },
    visiblePokemons() {
      return this.filteredPokemons.filter(pokemon => pokemon.id >= this.visibleRange.start && pokemon.id <= this.visibleRange.end);
    }
  },
  methods: {
    toggleFavorite(pokemon) {
      pokemon.favorite = !pokemon.favorite;
      if (pokemon.favorite) {
        this.favorites.push(pokemon.id);
      } else {
        const index = this.favorites.indexOf(pokemon.id);
        if (index !== -1) this.favorites.splice(index, 1);
      }
    },
    toggleFavorites() {
      this.showFavorites = !this.showFavorites;
    },
    filterByType(type) {
      this.selectedType = type;
    },
    updateVisibleRange(range) {
      this.visibleRange = range;
    },
    fetchPokemonData() {
      fetch('https://pokeapi.co/api/v2/pokemon?limit=151')
        .then(response => response.json())
        .then(data => {
          const pokemonPromises = data.results.map(pokemon => {
            return fetch(pokemon.url)
              .then(response => response.json())
              .then(pokeData => ({
                id: pokeData.id,
                name: pokeData.name,
                imageUrl: pokeData.sprites.other['official-artwork'].front_default,
                types: pokeData.types.map(type => type.type.name),
                favorite: false
              }));
          });
          Promise.all(pokemonPromises)
            .then(pokemons => {
              this.pokemons = pokemons;
              this.allTypes = [...new Set(pokemons.flatMap(pokemon => pokemon.types))];
            })
            .catch(err => console.error("Error fetching Pokemon data: ", err));
        })
        .catch(err => console.error("Error fetching API: ", err));
    }
  },
  created() {
    this.fetchPokemonData();
  }
};
</script>

<style scoped>
.container {
  padding: 20px;
  min-height: 80vh; /* Ensures the container takes at least 80% of the viewport height */
}

.pokemon-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  min-height: 50vh; /* Ensures the grid takes at least 50% of the viewport height */
}

.pokemon-card {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
  min-width: 200px; /* Ensures a minimum width for each card */
  max-width: 200px; /* Ensures a maximum width for each card */
  flex: 1 1 200px; /* Ensures the cards are flexible but maintain a minimum width */
}

.img-fluid {
  max-width: 100%;
  height: auto;
}
</style>
