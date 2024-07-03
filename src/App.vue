
<template>
  <div id="app">
    <img height="300" src="./assets/title.png" />
    <h1>¿Quién es ese Pokémon?</h1>
    <h3>
      Pokemones descubiertos: <span>{{ pokemonesDescubiertos }}</span>
    </h3>
    <div class="grid">
      <!-- Itera sobre el array de pokemones para crear una tarjeta para cada uno -->
      <div class="card" v-for="(pokemon, i) in pokemones" :key="i">
        <!-- Componente hijo PokemonCard, pasando props y escuchando evento -->
        <PokemonCard :pokemon="pokemon" @descubrirPokemon="descubrirPokemon" :index="i"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  name: 'App',
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemones: [],
    };
  },
  computed: {
    pokemonesDescubiertos() {
      return this.pokemones.filter(pokemon => pokemon.descubierto).length;
    },
  },
  methods: {
    async getPokemones() {
      const response = await axios.get('https://pokeapi.co/api/v2/pokemon');
      this.pokemones = response.data.results.map(pokemon => ({
        ...pokemon,
        descubierto: false,
      }));
    },
    descubrirPokemon({ inputName, index }) {
      if (inputName.toLowerCase() === this.pokemones[index].name) {
        this.pokemones[index].descubierto = true;
      }
    },
  },
  mounted() {
    this.getPokemones();
  },
};
</script>

<style>
#app {
  padding: 20px;
  text-align: center;
  font-family: sans-serif;
}

.grid {
  margin-top: 40px;
  display: grid; 
  grid-template-columns: repeat(4, 1fr); 
  gap: 20px; 
}

h3 {
  span {
    color: #ffcb03; 
    text-shadow: 0px 0px 2px #095a9a; 
  }
}
</style>
