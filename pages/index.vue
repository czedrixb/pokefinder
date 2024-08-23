<template>
  <div>
    <h1 class="text-5xl uppercase text-secondary bg-primary mb-5 text-center">
      pokéfinder
    </h1>

    <div class="bg-white flex items-center rounded-full shadow mb-20">
      <input
        type="text"
        class="w-full py-2 px-4 text-gray-700 focus:outline-none"
      />
      <div class="p-2">
        <button
          class="rounded-full focus:outline-none w-10 h-12 md:w-10 md:h-12 flex items-center justify-center"
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
      <div
        v-for="n in 5"
        :key="n"
        class="border rounded-lg py-1 px-2 bg-white h-full w-full"
      >
        <div class="mb-3">
          <img
            src="https://img.pokemondb.net/artwork/avif/bulbasaur.avif"
            class="w-full"
            alt=""
          />
        </div>

        <div class="mb-3">
          <div class="text-secondary">#0001</div>
          <div class="text-secondary capitalize">baulbasaur</div>
        </div>

        <div class="flex flex-wrap gap-2">
          <div
            class="rounded-full py-1 px-2 bg-grass text-center uppercase text-white text-xs"
          >
            grass
          </div>
          <div
            class="rounded-full py-1 px-2 bg-poison text-center uppercase text-white text-xs"
          >
            poison
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  head() {
    return {
      title: "Pokéfinder",
    };
  },
};
</script>
