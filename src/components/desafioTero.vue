<template>
  <v-container id="dropdown-example">
    <v-parallax dark src="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg">
      <v-row align="center" justify="center">
        <h1 class="display-2 font-weight-thin mb-4">Tabela FIPE</h1>
        <h4 class="subheading">Consulte o preço de carros e motos novos e usados</h4>
      </v-row>
    </v-parallax>

    <v-row>
      <v-col cols="8">
        <p>Tipo de veiculo</p>

        <v-select
          class="my-2"
          :items="dropdown_tipo"
          item-text="name"
          item-value="id"
          v-model="tipo"
          v-on:change="getMarca"
          target="#dropdown-example"
        ></v-select>
      </v-col>
      <v-col cols="8">
        <p>Marca</p>

        <v-select
          class="my-2"
          :items="dropdown_marca"
          item-text="name"
          item-value="id"
          v-model="marca"
          v-on:change="getModelo"
          target="#dropdown-example"
          editable
        ></v-select>
      </v-col>

      <v-col cols="8">
        <p>Modelo</p>

        <v-select
          class="my-2"
          :items="dropdown_modelo"
          item-text="name"
          item-value="id"
          v-model="modelo"
          v-on:change="getAno"
          editable
        ></v-select>
      </v-col>

      <v-col cols="8">
        <p>Ano</p>

        <v-select
          class="my-2"
          :items="dropdown_ano"
          item-text="id"
          item-value="id"
          v-model="ano"
          v-on:change="getCombustivel"
          editable
        ></v-select>
      </v-col>

      <v-col cols="8">
        <p>Combustivel</p>

        <v-select
          class="my-2"
          :items="dropdown_combustivel"
          v-on:change="getValor"
          target="#dropdown-example"
        ></v-select>
      </v-col>
    </v-row>
    <v-btn color="blue" dark @click="mostrarValor()">Consultar Preço</v-btn>
    <v-bottom-sheet v-model="mostrando">
      <v-sheet class="text-center" height="200px">
        <v-btn class="mt-6" text color="red" @click="mostrando = !mostrando">Fechar</v-btn>
        <div class="py-3">{{valor}}</div>
      </v-sheet>
    </v-bottom-sheet>
    <v-footer>
      <div class="flex-grow-1"></div>
      <div>&copy; {{ new Date().getFullYear() }}</div>
    </v-footer>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    dropdown_tipo: [],
    dropdown_marca: [],
    dropdown_ano: [],
    dropdown_modelo: [],
    dropdown_combustivel: [],
    tipo: null,
    marca: null,
    modelo: null,
    ano: null,
    mostrando: false,
    valor: ""
  }),
  methods: {
    getTipo: function() {
      this.dropdown_tipo = [
        { name: "Carro", id: "carros" },
        { name: "Moto", id: "motos" }
      ];
    },
    getMarca: function() {
      axios
        .get(`https://fipeapi.appspot.com/api/1/${this.tipo}/marcas.json`)
        .then(response => {
          this.dropdown_marca = response.data;
          this.dropdown_modelo = [];
          this.dropdown_ano = [];
          this.dropdown_combustivel = [];
          this.valor = "";
        });
    },
    getModelo: function() {
      axios
        .get(
          `https://fipeapi.appspot.com/api/1/${this.tipo}/veiculos/${this.marca}.json`
        )
        .then(response => {
          this.dropdown_modelo = response.data;
          this.dropdown_ano = [];
          this.dropdown_combustivel = [];
          this.valor = "";
        });
    },
    getAno: function() {
      axios
        .get(
          `https://fipeapi.appspot.com/api/1/${this.tipo}/veiculo/${this.marca}/${this.modelo}.json`
        )
        .then(response => {
          this.dropdown_ano = response.data;
          this.dropdown_combustivel = [];
          this.valor = "";
        });
    },
    getCombustivel: function() {
      this.dropdown_combustivel = ["Gasolina"];
    },
    getValor: function() {
      axios
        .get(
          `https://fipeapi.appspot.com/api/1/${this.tipo}/veiculo/${this.marca}/${this.modelo}/${this.ano}.json`
        )
        .then(response => {
          this.valor = response.data.preco;
        });
    },
    mostrarValor: function() {
      if (this.valor == "") {
        return;
      }
      this.mostrando = !this.mostrando;

    }
  },
  mounted() {
    this.getTipo();
  }
};
</script>

