<template>
  <div>
    <form @submit.prevent="cadastrar">
      <h2>Moto</h2>
      <div class="form-group">
        <label for="placa">Placa</label>
        <input
          type="text"
          id="placa"
          class="form-control"
          required
          autofocus
          v-model="placa"
        />
      </div>
      <div class="form-group">
        <label for="modelo">Modelo</label>
        <textarea id="modelo" class="form-control" required v-model="modelo">
        </textarea>
      </div>
      <button class="btn btn-lg btn-primary btn-block" type="submit">
        Salvar
      </button>
    </form>
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Placa</th>
          <th>Modelo</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="mto in motos" :key="mto.id">
          <td>{{ mto.id }}</td>
          <td>{{ mto.placa }}</td>
          <td>{{ mto.modelo }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
import { mapState } from "vuex";

export default {
  name: "motos",
  data() {
    return {
      placa: "",
      modelo: "",
      motos: [],
    };
  },
  computed: {
    ...mapState(["usuario"]),
  },
  methods: {
    cadastrar() {
      axios
        .post("moto", {
          placa: this.placa,
          modelo: this.modelo,
        })
        .then((res) => {
          console.log(res);
          this.placa = "";
          this.modelo = "";
          this.atualizar();
        })
        .catch((error) => console.log(error))
    },
    atualizar() {
      axios
        .get("moto", { headers: { Accept: "application/json" } })
        .then((res) => {
          console.log(res);
          this.motos = res.data;
        })
        .catch((error) => console.log(error));
    },
  },
  created() {
    this.atualizar();
  },
};
</script>
