<template>
  <div id="app" class="container">
    <div>
      <img class="imgLogo my-2" src="./assets/logo.png" alt="Pokémon" />
      <h1 class="fw-bold my-2">¿Quién es ese Pokémon?</h1>
    </div>
    <div class="d-flex justify-content-center align-items-center">
      <p class="m-2">Pokemones descubiertos</p>
      <span class="m-2 text-center" id="totalPoke">
        <!-- <img class="cuentaPoke" src="./assets/pokebola.png" /> -->
        <strong>{{ this.contador }}</strong>
      </span>
    </div>
    <div class="row py-3">
      <div class="col-3" v-for="(pokemon, index) in pokemones" :key="index">
        <CardPokemon
          :imgPoke="pokemon.image"
          :nombrePoke="pokemon.name"
          @descubrir="mostrarPoke()"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CardPokemon from "./components/CardPokemon.vue";

export default {
  name: "App",
  components: {
    CardPokemon,
  },
  data() {
    return {
      pokemones: [],
      contador: "0",
    };
  },
  methods: {
    mostrarPoke: function () {
      this.contador++;
    },
  },
  async mounted() {
    try {
      const getData = async function (url) {
        let response = await axios.get(url);
        return {
          image: response.data.sprites.other.dream_world.front_default,
          name: response.data.name,
        };
      };
      let response = await axios.get("https://pokeapi.co/api/v2/pokemon/");
      let { data } = response;
      Promise.all(data.results.map((pokemon) => getData(pokemon.url)))
        .then((pokemones) => (this.pokemones = pokemones))
        .catch((error) => {
          alert("Error al procesar pokemones.");
          console.log(error);
        });
    } catch (error) {
      console.log(error);
      if (error.code == "ERR_NETWORK") {
        return alert(
          "En estos momento el servidor está caído, puede intentar más tarde."
        );
      }
      if (error.response.status == 404) {
        alert("El recurso que está buscando no existe.");
      } else {
        alert("El servidor en estos momentos no puede procesar su solicitud.");
      }
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #2c3e50;
  margin-top: 60px; */
}
.imgLogo {
  width: 40vw;
  max-height: 100%;
}
#totalPoke {
  /* background-color: yellow; */
  color: yellow;
}
.cuentaPoke {
  max-width: 75px;
}
</style>
