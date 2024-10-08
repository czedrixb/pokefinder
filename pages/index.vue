<template>
  <div>
    <h1 class="text-5xl uppercase text-secondary mb-5 text-center">
      pokéfinder
    </h1>

    <div
      class="bg-white border flex items-center rounded-full shadow mb-20"
      :class="searchFocus"
    >
      <input
        type="text"
        v-model="search"
        placeholder="Search Pokémon..."
        class="w-full py-2 px-4 text-gray-700 bg-transparent focus:outline-none"
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

    <div v-if="preload" class="flex justify-center items-center">
      <img
        src="https://i.imgur.com/K6vsFTX.gif"
        class="pokeball w-20"
        alt="pokeball"
      />
    </div>

    <div v-else class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-4">
      <button
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

<script setup>
import { ref, onMounted, computed } from "vue";
import axios from "axios";
import Pokemon from "@/components/pokemon.vue";

// mega
useSeoMeta({
  title: "Pokéfinder - Home",
  ogTitle: "Pokéfinder - Home",
  description: "This is like a pokédex for pokemons.",
  ogDescription: "This is like a pokédex for pokemons.",
});

// Reactive state
const modal = ref(false);
const preload = ref(true);
const pokemons = ref([]);
const search = ref("");

// Fetch Pokémon data
const getPokemon = async () => {
  try {
    const response = await axios.get("https://pokeapi.co/api/v2/pokemon/");
    const pokemonList = response.data.results;

    const detailedPokemonRequests = pokemonList.map((pokemon) =>
      axios.get(pokemon.url)
    );
    const detailedPokemons = await Promise.all(detailedPokemonRequests);

    pokemons.value = detailedPokemons.map((pokemonResponse) => {
      const pokemonData = pokemonResponse.data;
      return {
        id: pokemonData.id,
        name: pokemonData.name,
        image: pokemonData.sprites.front_default,
        type1: pokemonData.types[0].type.name,
        type2: pokemonData.types[1] ? pokemonData.types[1].type.name : null,
      };
    });

    preload.value = false;
  } catch (error) {
    console.error("Error fetching Pokémon:", error);
  }
};

// Computed properties
const filteredPokemons = computed(() => {
  const term = search.value.toLowerCase();
  return pokemons.value.filter(
    (pokemon) =>
      pokemon.name.toLowerCase().includes(term) ||
      pokemon.id.toString().includes(term)
  );
});

const searchFocus = computed(() => {
  return search.value ? `border-grass shadow-md` : `border-gray-300`;
});

// Lifecycle hooks
onMounted(() => {
  getPokemon();
});

// Methods
const getType = (type) => {
  return `bg-${type}`;
};
</script>
