<script>
import { mapStores, mapState, mapActions } from "pinia";
import { useAuthStore } from "@/stores/auth";
export default {
  data() {
    return {
      username: "",
      password: "",
    };
  },
  async created() {
    await this.token();
  },
  computed: {
    ...mapStores(useAuthStore),
    ...mapState(useAuthStore, ["userData"]),
    ...mapState(useAuthStore, ["userToken"]),
  },
  methods: {
    ...mapActions(useAuthStore, ["token", "login"]),
    async submit() {
      try {
        await this.login(this.username, this.password);
        this.$router.push("/filme");
      } catch (e) {
        this.$toast.error(`usuario ou senha invalido`, {
          type: "error",
          position: "top-right",
        });
        setTimeout(this.$toast.clear, 3000);
      }
    },
  },
};
</script>
<template>
  <div class="conteudo-login">
    {{ userData.name }}
    <form @submit.prevent="submit">
      <div class="centralizando">
        <div class="forms-login">
          <div class="titulo-login">
            <h1>Filme Show</h1>
          </div>
          <div class="infos">
            <label for="">Usuario</label>
            <input class="input-login" v-model="username" type="text" />
          </div>
          <div class="infos">
            <label for="">Senha</label>
            <input class="input-login" v-model="password" type="password" />
            <button class="btn-entrar" type="submit">Entrar</button>
          </div>
          <!-- <div class="cadastro">
            <p>
              Não possui um conta?
              <a href="https://www.themoviedb.org/" target="_blank"
                >Cadastre-se</a
              >   NÃO SEI O PQ KRLS ESSA MERDA TA FICANDO TORTA, TALVEZ SEJA EU 
            </p>
          </div> -->
        </div>

        <div v-if="userToken.false">
          <p>
            {{ userToken.status_message }}
          </p>
        </div>
      </div>
    </form>
  </div>
</template>
