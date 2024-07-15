<template>
  <div class="pokemon-container">
    <img :src="imgSrc"
      alt="Pokemon" class="hidden-pokemon" />
    <img
      v-if="showPokemon"
      :src="imgSrc"
      alt="Pokemon"
      class="fade-in" 
    /> 
  </div>
</template>

<script>
export default {
  props: {
    pokemonId: {
      type: Number,
      required: true,
    },
    showPokemon: {
      type: Boolean,
      required: true,
      default: false,
    }
  },
  data() {
    return {};
  },
  computed: {
    imgSrc() {
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.pokemonId}.svg`;
    },
  }
}
</script>

<style scoped>
/* Pokemon Picture */
.pokemon-container {
  position: relative;
  height: 200px;
  width: 200px;
  /* background-color: red; */
}

img {
  height: 200px;
  position: absolute;
  right: 5px;
  padding-top: 4px;
  padding-bottom: 4px;
  /* right: 32%; */
  /* top: 0;
  bottom: 0;
  right: 0;
  left: 0; */
  /* margin: auto; */
  /* transition: opacity 0.5s; */
  user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -webkit-user-drag: none;
  -webkit-user-select: none;
}

.hidden-pokemon {
  filter: brightness(0);
}
</style>