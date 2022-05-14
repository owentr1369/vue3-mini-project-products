<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>Home</h1>
  </div>
</template>

<script>
// @ is an alias to /src
import { reactive } from "vue";

export default {
  name: "HomeView",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
    });
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
  },
};
</script>
