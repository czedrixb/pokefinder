<template>
  <div>
    <h1 class="text-5xl uppercase text-secondary mb-5 text-center">
      pokéfinder
    </h1>

    <div class="bg-white border flex items-center rounded-full shadow mb-20">
      <input
        type="text"
        v-model="searchTerm"
        placeholder="Search Pokémon..."
        class="w-full py-2 px-4 text-gray-700 focus:outline-none"
      />
      <div class="p-2">
        <button
          class="rounded-full focus:outline-none w-10 h-12 md:w-10 md:h-12 flex items-center justify-center animate-rotate"
        >
          <img
            src="https://github.com/ahampriyanshu/gokemon/raw/master/assets/img/pokeball.png"
            class="pokeball"
            alt="pokeball"
          />
        </button>
      </div>
    </div>

    <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-4">
      <button
        @click="modal = true"
        v-for="pokemon in filteredPokemons"
        :key="pokemon.name"
        class="border border-2 text-left rounded-lg hover:border-grass focus:border-grass transition-colors ease-in-out p-4 bg-white h-full w-full"
      >
        <div class="mb-7 flex justify-center">
          <img :src="pokemon.image" class="w-full" alt="" />
        </div>

        <div class="mb-3">
          <div class="text-secondary">#{{ pokemon.id }}</div>
          <div class="text-secondary capitalize">{{ pokemon.name }}</div>
        </div>

        <div class="flex flex-wrap gap-2">
          <div
            class="rounded-full py-1 px-2 text-center text-black uppercase text-white text-xs"
            :class="getType(pokemon.type1)"
          >
            {{ pokemon.type1 }}
          </div>
          <div
            v-if="pokemon.type2 != null"
            class="rounded-full py-1 px-2 text-center text-white uppercase text-white text-xs"
            :class="getType(pokemon.type2)"
          >
            {{ pokemon.type2 }}
          </div>
        </div>
      </button>
    </div>

    <Pokemon :modal="modal" @close="modal = false" />
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "@/components/pokemon.vue";

export default {
  components: {
    Pokemon,
  },

  data() {
    return {
      modal: false,
      pokemons: [],
      searchTerm: "",
    };
  },

  mounted() {
    this.getPokemon();
  },

  computed: {
    filteredPokemons() {
      const term = this.searchTerm.toLowerCase();
      return this.pokemons.filter(
        (pokemon) =>
          pokemon.name.toLowerCase().includes(term) ||
          pokemon.id.toString().includes(term)
      );
    },
  },

  methods: {
    async getPokemon() {
      try {
        const response = await axios.get("https://pokeapi.co/api/v2/pokemon/");
        const pokemonList = response.data.results;

        const detailedPokemonRequests = pokemonList.map((pokemon) =>
          axios.get(pokemon.url)
        );
        const detailedPokemons = await Promise.all(detailedPokemonRequests);

        this.pokemons = detailedPokemons.map((pokemonResponse) => {
          const pokemonData = pokemonResponse.data;
          return {
            id: pokemonData.id,
            name: pokemonData.name,
            image: pokemonData.sprites.front_default,
            type1: pokemonData.types[0].type.name,
            type2: pokemonData.types[1] ? pokemonData.types[1].type.name : null,
          };
        });
      } catch (error) {
        console.error("Error fetching Pokémon:", error);
      }
    },

    getType(type) {
      return `bg-${type}`;
    },
  },
};
</script>
