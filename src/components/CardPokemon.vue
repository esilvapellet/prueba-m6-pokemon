<template>
  <div class="CardPokemon">
    <div class="card m-3 p-2 border-primary-subtle" style="width: 18rem">
      <img
        class="card-img-top imgPoke"
        :src="imgPoke"
        :class="{ visible: !mostrar }"
      />
      <div class="card-body">
        <div v-if="mostrar">
          <!-- <div v-if="mostrar"> -->
          <h5 class="card-title">
            <strong>{{ nombrePoke }}</strong>
          </h5>
        </div>
        <div v-else>
          <div class="card-text py-3">
            <input class="text-center" type="text" v-model="respuestaPoke" />
          </div>
          <button class="btn btn-secondary" @click="enviarPoke">
            Descubrir
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardPokemon",
  props: {
    imgPoke: String,
    nombrePoke: String,
  },
  data() {
    return {
      respuestaPoke: "",
      mostrar: false,
    };
  },
  methods: {
    enviarPoke: function () {
      if (this.nombrePoke === this.respuestaPoke) {
        this.$emit("descubrir");
        this.mostrar = !this.mostrar;
      } else {
        alert(`Pista: El nombre comienza con ${this.nombrePoke.charAt(0)} ...`);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.imgPoke {
  max-height: 100px;
}
.visible {
  filter: blur(5px) grayscale(100%);
}
</style>
