<template>
  <form @submit.prevent="login">
    <h2>Login</h2>
    <div class="form-group">
      <label for="username">Usuário</label>
      <input
        type="text"
        id="username"
        class="form-control"
        required
        autofocus
        v-model="nome"
      />
    </div>
    <div class="form-group">
      <label for="inputPassword">Senha</label>
      <input
        type="password"
        id="inputPassword"
        class="form-control"
        required
        v-model="senha"
      />
    </div>
    <button class="btn btn-lg btn-primary btn-block" type="submit">
      Login
    </button>
  </form>
</template>

<script>
import axios from "axios";
import { mapMutations } from "vuex";
import { mapState } from "vuex";

export default {
  name: "login",
  data() {
    return {
      nome: "",
      senha: "",
    };
  },
  computed: {
    ...mapState(["papel"]),
    papel: {
      get() {
        return this.$store.state.papel;
      },
      set(value) {
        this.setPapel(value);
      },
    },
  },
  methods: {
    ...mapMutations(["setUsuario", "setToken", "setPapel"]),
    login() {
      axios
        .post("login", {
          username: this.nome,
          password: this.senha,
        })
        .then((res) => {
          console.log(res);
          this.setUsuario(res.data);
          this.setToken(res.data.token);
          this.setPapel(res.data.autorizacao);
          this.sucesso();
        })
        .catch((error) => {
          if (error.response.status === 403) {
            var msg = "Usuário ou senha inválida !";
            alert(msg);
          }
        });
    },
    sucesso() {
      if (this.papel === "ROLE_ADMIN") {
        this.$router.push("/usuario");
      } else if (this.papel === "ROLE_USUARIO") {
        this.$router.push("/motos");
      }
    },
  },
};
</script>