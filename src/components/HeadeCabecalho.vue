<script>
import { mapStores, mapState, mapActions } from "pinia";
import { useGenreStore } from "@/stores/genres";
import { useAuthStore } from "@/stores/auth";
export default {
  data() {
    return {
      genero: "",
    };
  },
  async created() {
    this.generos = await this.getAllGenres();
  },
  // setup(){
  //    const authStore = useAuthStore();
  //    const { userData: authuser } = storeToRefs(authStore);
  //  },
  computed: {
    ...mapStores(useAuthStore),
    ...mapStores(useGenreStore),
    ...mapState(useAuthStore, ["userData"]),
    ...mapState(useGenreStore, ["genres"]),
  },
  methods: {
    ...mapActions(useGenreStore, ["getAllGenres"]),
    go() {
      this.$router.push(`/filmes_por_genero/${this.genero}`);
    },
  },
};
</script>

<template>
  <header>
    <div class="cabecalho">
      <div>
        <span class="cabecalho-span">
          <RouterLink to="/filme">FilmeShow</RouterLink>
        </span>
        <span>
          <select v-model="genero" @change="go" class="select-cabecalho">
            <option value="" disabled>Categorias</option>
            <option
              v-for="genero of genres"
              :key="genero.id"
              :value="genero.id"
            >
              {{ genero.name }}
            </option>
          </select>
        </span>
        <span class="cabecalho-span"
          ><RouterLink to="/lancamento">Lançamentos</RouterLink></span
        >
      </div>
      <div>
        <RouterLink to="/minha-conta"
          ><span class="cabecalho-span">{{
            userData.username
          }}</span></RouterLink
        >
        <button>
          <RouterLink to="/pesquisa" class="button-pesquisar"
            >Pesquisar
          </RouterLink>
        </button>
      </div>
    </div>
  </header>
</template>
