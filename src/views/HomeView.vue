<template>
  <div class="w-full flex justify-center">
    <img alt="Vue logo" src="../assets/logo.png" />
  </div>
  <div class="w-full flex justify-center">
    <input
      type="text"
      placeholder="Enter Pokemon here"
      class="mt-2 p-2 border-orange-500 border-2"
      v-model="text"
    />
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div
      v-for="(pokemon, idx) in filteredPokemons"
      :key="idx"
      class="ml-4 text-2x text-blue-200"
    >
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}</router-link
      >
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { computed, reactive, toRefs } from "vue";

export default {
  name: "HomeView",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: "",
      filteredPokemons: computed(() => updatePokemon()),
    });
    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }
    fetch("https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0")
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),

          {}
        );
      });
    return { ...toRefs(state) };
  },
};
</script>
