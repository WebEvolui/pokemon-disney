<template>
  <b-container>
    <b-row class="text-center mt-5">
      <div class="input-group field-search">
        <input
          type="text"
          class="form-control"
          placeholder="Pesquise por nome ou id de um pokémon"
          aria-label="Pesquisa por nome ou ID de um pokemon e veja se existe um personagem na disney com o id dele"
          aria-describedby="button-addon4"
          v-model="txtPesquisar"
          @keypress.enter="pesquisar()"
        />
        <div class="input-group-append" id="button-addon4">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="pesquisar()"
          >
            Pesquisar
          </button>
        </div>
      </div>
    </b-row>

    <b-row class="text-center mt-3" align-h="center">
      <b-col align-h="center" class="mt-1">
        <h1 class="text-capitalize name-pokemon">{{ pokemon.name }}</h1>
        <div class="card-pokemon">
          <img
            class="img-fluid h-100"
            :src="pokemon.sprites.other.dream_world.front_default"
            :alt="pokemon.name"
          />
        </div>
        <span class="mt-1"
          >id <strong>{{ pokemon.id }}</strong> na Pokédex</span
        >
      </b-col>

      <b-col v-if="disney" align-h="center" class="mt-1">
        <h1 class="text-capitalize name-pokemon">{{ disney.name }}</h1>
        <div class="card-pokemon">
          <img
            class="img-fluid h-100"
            :src="disney.imageUrl"
            :alt="disney.name"
          />
        </div>
        <span class="mt-1"
          >id <strong>{{ disney._id }}</strong> na <strong>Disney</strong></span
        >
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "slug",
  loading: false,
  data() {
    return {
      pokemon: {},
      disney: null,
      txtPesquisar: null,
    };
  },
  async asyncData({ $axios, params }) {
    const pokemon = await $axios.$get("/pokemon/" + params.slug);
    return { pokemon };
  },
  mounted() {
    this.$nextTick(async () => {
      this.$nuxt.$loading.start();

      const disney = await this.$axios.$get(
        "https://api.disneyapi.dev/characters/" + this.pokemon.id
      );

      this.disney = disney;

      this.$nuxt.$loading.finish();
    });
  },
  methods: {
    pesquisar: function () {
      this.$router.push({ path: this.txtPesquisar });
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@100;400;900&display=swap");

body {
  font-family: "Inter", sans-serif;
  background-color: #f1f5f9;
}

.name-pokemon {
  text-shadow: 4px 4px 4px #aaa;
}

.card-pokemon {
  height: 300px;
}

.field-search {
    padding: 10px;
}

.card-body .card-title {
  text-transform: capitalize;
}
</style>
