<template>
  <div class="card">
    <img height="200" :src="pokemon.url" :class="{ filtered: !pokemon.descubierto }"/>
    <h2 v-show="pokemon.descubierto">{{ pokemon.name }}</h2>
    <div v-show="!pokemon.descubierto">
      <input v-show="!pokemon.descubierto" v-model="inputName" @keypress.enter="$emit('descubrirPokemon', { inputName, index })"/>
      <button v-show="!pokemon.descubierto" @click="$emit('descubrirPokemon', { inputName, index })">Descubrir Pokémon</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokemonCard',
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      inputName: '',
    };
  },
  methods: {
    descubrirPokemon() {
      this.$emit('descubrirPokemon', { inputName: this.inputName, index: this.index });
      this.inputName = '';
    },
  },
};
</script>

<style scoped>
.card {
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  text-align: center;
}
.filtered {
  filter: blur(5px);
}
</style>