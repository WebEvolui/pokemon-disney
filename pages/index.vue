<template>
  <b-container>
    <b-row class="text-center mt-5" align-h="center">
      <span
        >Parece que você não sabe onde está ... Tente isso
        <button
          class="btn btn-outline-primary"
          type="button"
          @click="iniciar()"
        >
          Iniciar
        </button></span
      >
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "inicio",
  methods: {
    iniciar: function () {
      this.$router.push({ path: "pikachu" });
    },
  },
};
</script>
