<template>
  <h1 v-if="!pokemon">Espere por favor..</h1>
  <div v-else>
    <h1>¿Quién es este Pokémon?</h1>

    <PokemonPicture
      :pokemon-id="pokemon.id"
      :show-shadow="showShadow"
      :show-pokemon="showPokemon"
    />
    <PokemonOptions
      :pokemons="pokemonArr"
      :election="election"
      @selection="checkAnswer"
    />
    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">Nuevo Juego</button>
    </template>
  </div>

  <!-- TODO: img -->

  <!-- TODO: Opciones -->
</template>

<script>
import PokemonOptions from "@/components/PokemonOptions.vue";
import PokemonPicture from "@/components/PokemonPicture.vue";

import getPokemonOptions from "@/helpers/getPokemonOptions.js";
console.log(getPokemonOptions());

export default {
  components: { PokemonOptions, PokemonPicture },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showShadow: true,
      showAnswer: false,
      election: false,
      message: null,
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();

      const rndInt = Math.floor(Math.random() * 4);

      this.pokemon = this.pokemonArr[rndInt];
    },
    checkAnswer(selectedId) {
      this.showShadow = false;
      this.showPokemon = true;
      this.showAnswer = true;
      this.election = true;

      if (selectedId === this.pokemon.id) {
        this.message = `Correcto, era ${this.pokemon.name}!`;
      } else {
        this.message = `Error, era ${this.pokemon.name} =(`;
      }
    },
    newGame() {
      this.showShadow = true;
      this.showPokemon = false;
      this.showAnswer = false;
      this.election = false;
      this.pokemonArr = [];
      this.pokemon = null;
      this.mixPokemonArray();
    },
  },
  mounted() {
    this.mixPokemonArray();
  },
};
</script>
