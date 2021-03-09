<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/beforeLogo.png" />

      <hr />
      <h4 class="is-size">Pokedex Before</h4>
      <input
        type="text"
        id="Buscar Pokemon Nome"
        v-model="textoBuscado"
        class="input is-rounded"
      />

      <div class="columns is-multiline is-mobile is-centered">
        <div
          class="column is-one-third"
          v-for="(poke, index) in resultadoBusca"
          :key="index"
        >
          <Pokemon
            :textoBuscado="textoBuscado"
            :name="poke.name"
            :url="poke.url"
            :num="poke.num"
            :type="poke.type"
            :abilities="poke.name"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      busca: "",
    };
  },
  created: function() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Carregou a lista de pokemons :)");
        const pokemonsWithId = res.data.results;
        this.pokemons = pokemonsWithId.map((item, index) => {
          return { ...item, num: index + 1 };
        });
      });
  },
  components: {
    Pokemon,
  },
  computed: {
    textoBuscado: {
      get() {
        return this.busca;
      },
      set(value) {
        this.busca = value;
      },
    },

    resultadoBusca: function() {
      if (this.busca == "" || this.busca == " ") {
        return this.pokemons;
      } else {
        let busca = this.pokemons.filter(
          ({ name, num }) =>
            name === this.busca || Number(num) === Number(this.busca)
        );
        console.log(busca);
        return busca;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
