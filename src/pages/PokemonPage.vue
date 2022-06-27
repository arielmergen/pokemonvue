<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  <template v-else>
    <h1>¿Quien es este Pokemon?</h1>
    <PokemonPicture :pokemon-id="pokemon.id" :show-pokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAnswer" />
    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">Nuevo Juego</button>
    </template>
  </template>
</template>

<script>
  import PokemonPicture from '@/components/PokemonPicture';
  import PokemonOptions from '@/components/PokemonOptions';

  import getPokemonOptions from '@/helpers/getPokemonOptions';
  import { numberTypeAnnotation } from '@babel/types';
  export default {
    components: { PokemonPicture, PokemonOptions },
    data() {
      return {
        pokemonArr: [],
        pokemon: null,
        showPokemon: false,
        showAnswer: false,
        message: '',
      };
    },
    methods: {
      async mixPokemonArray() {
        this.pokemonArr = await getPokemonOptions();
        const rndInt = Math.floor(Math.random() * 4);
        this.pokemon = this.pokemonArr[rndInt];
      },
      checkAnswer(pokemonid) {
        this.isPokemon(pokemonid);
      },
      isPokemon(selectedId) {
        this.showPokemon = true;
        this.showAnswer = true;
        if (this.pokemon.id === selectedId) {
          this.message = `Correcto ${this.pokemon.name}`;
        } else {
          this.message = `Oops, era ${this.pokemon.name}`;
        }
      },
      newGame() {
        this.showPokemon = false;
        this.showAnswer = false;
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
