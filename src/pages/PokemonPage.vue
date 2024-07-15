<template>
  <h1 v-if="!pokemon" class="loading" >Đang tải........</h1>
  <div v-else class="pokemon-page">
    <h1 class="title_pokemon">Pokémon này là ai?</h1>
    <div class="score-container">
      <h3>Trả lời đúng liên tục: {{ score }} câu</h3>
      <h3>Tổng câu trả lời đúng: {{ maxScore }} câu</h3>
    </div>
    <div>
      <button @click="resetScores" class="resetscores">Reset Game</button>
    </div>
    <PokemonPicture :pokemon-id="pokemon.id" :show-pokemon="showPokemon" />
    <h2 v-if="showAnswer" :class="['fade-in', answerClass].join(' ')">{{ message }}</h2>
    <pokemonOptions
      :pokemons="pokemonArr"
      :pokemon="pokemon"
      :optionSelected="optionSelected"
      @select-pokemon="checkAnswer" 
    />

    <button @click="newGame" v-if="showAnswer" class="fade-in">
      {{ optionSelected !== pokemon.id ? 'Trả lời câu hỏi tiếp theo' : 'Tiếp tục' }}
    </button>
  </div>
  <PokemonLogo/>
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions.vue';
import PokemonPicture from '@/components/PokemonPicture.vue';
import PokemonLogo from '@/components/PokemonLogo.vue';
import getPokemonOptions from '@/helpers/getPokemonOptions';

export default {
  components: {
    PokemonOptions,
    PokemonPicture,
    PokemonLogo,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: '',
      optionSelected: null,
      score: 0,
      maxScore: 0,
    };
  },
  methods: {
    resetScores() {
      this.score = 0;
      this.maxScore = 0;
    },

    async mixPokemonOptions() {
      this.pokemonArr = await getPokemonOptions();

      const randomIndex = Math.floor(Math.random() * this.pokemonArr.length);
      this.pokemon = this.pokemonArr[randomIndex];
    },
    checkAnswer(pokemonIdSelected) {
      this.showPokemon = true
      this.optionSelected = pokemonIdSelected;

      if (pokemonIdSelected === this.pokemon.id) {
        this.message = `Chính xác! đó là Pokémon "${this.pokemon.name}".`
        this.score += 1;
        if (this.score > this.maxScore) {
          this.maxScore = this.score;
          localStorage.setItem('maxScore', this.maxScore);
        }
      } else {
        this.message = `Bạn đã chọn sai! Pokémon đó là: "${this.pokemon.name}".`
        // this.score = 0;
      }

      this.showAnswer = true;
    },
    newGame() {
      if (this.optionSelected !== this.pokemon?.id) this.score = 0;
      this.pokemon = null;
      this.optionSelected = null;
      this.showPokemon = false;
      this.showAnswer = false;
      this.message = '';
      this.pokemonArr = [];
      this.mixPokemonOptions();
    },
  },
  mounted() {
    this.mixPokemonOptions();
    this.maxScore = localStorage.getItem('maxScore') || 0;
  },
  computed: {
    answerClass() {
      if (!this.pokemon) return '';
      if (this.optionSelected === this.pokemon.id) return 'success';
      return 'error';
    },
  }
};

</script>

<style scoped>
.pokemon-page {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
}

.score-container {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin-top: 8px;
  margin-bottom: 20px;
}

h2 {
  margin-top: 16px;
  color: white;
  font-size: 18px;
  padding: 4px 16px 8px 16px;
  border-radius: 8px;
  /* background-color: #f44336; */
}

.success {
  background-color: #4caf50;
  /* border: 1px solid #4caf50; */
}

.error {
  background-color: #f44336;
  /* border: 1px solid #ef4444; */
}

button {
  background-color: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 12px 24px;
  font-size: 16px;
  cursor: pointer;
  margin-top: 20px;
}
button:hover {
  background-color: #2563eb;
}
.title_pokemon{
  font-size: 50px;
  font-weight: bold;
  margin-top: -20px;
  margin-bottom: 50px;
}
.loading{
  font-size: 50px;
  font-weight: bold;
}
.resetscores{
  position: absolute;
  margin-top: -105px;
  margin-left: -80px;
  font-weight: bold;
}
</style>