<template>
  <div id="app">
    <b-container fluid>
      <b-row class="my-1 center">
        <b-col sm="4">
          <b-form-input
            id="input-small"
            v-model="valor"
            size="sm"
            type="number"
            placeholder="Insira um valor"
          ></b-form-input>
        </b-col>
        <b-col sm="2"
          ><b-button variant="primary" @click="obterDivisoresPrimos()" size="sm"
            >Obter Divisores</b-button
          ></b-col
        >
      </b-row>
    </b-container>

    <b-container fluid>
      <b-row class="my-1 center mt-5">
        <b-col sm="3">
          <b-list-group>
            <label for="lista de divisores">Divisores</label>
            <b-list-group-item
              v-for="(divisor, index) in lstDivisores"
              :key="index"
            >
              <b-badge variant="primary" pill>{{ divisor }}</b-badge>
            </b-list-group-item>
          </b-list-group>
        </b-col>

        <b-col sm="3">
          <b-list-group>
            <label for="lista de dos divisores primos">Divisores Primos</label>
            <b-list-group-item
              v-for="(primos, index) in lstNumerosPrimos"
              :key="index"
            >
              <b-badge variant="primary" pill>{{ primos }}</b-badge>
            </b-list-group-item>
          </b-list-group>
        </b-col>
      </b-row>
    </b-container>

    <!--    Loading -->
    <div v-if="isLoading" class="text-center">
      <b-spinner
        style="position: absolute;top: 50%;left: 50%;"
        label="Spinning"
      ></b-spinner>
    </div>

    <Alert ref="alertComponent"></Alert>
  </div>
</template>

<script>
import Alert from "./components/Alert";
export default {
  name: "App",
  components: {
    Alert
  },
  data: () => ({
    lstDivisores: null,
    lstNumerosPrimos: null,
    valor: null,
    isLoading: false
  }),
  methods: {
    obterDivisoresPrimos() {
      this.isLoading = true;
      if (this.valor > 0) {
        const _this = this;
        const URL_TO_FETCH = `https://numbers-divisores-primos.herokuapp.com/api/v1/${this.valor}`;
        fetch(URL_TO_FETCH, {
          method: "get" // opcional
        })
          .then(function(response) {
            response.json().then(function(data) {
              const { lstDivisores, lstNumerosPrimos } = data;
              if (data) {
                _this.lstDivisores = lstDivisores;
                _this.lstNumerosPrimos = lstNumerosPrimos;
                _this.isLoading = false;
              }
            });
          })
          .catch(function(err) {
            _this.isLoading = false;
            console.error(err);
          });
      } else {
        this.$refs.alertComponent.showAlert(2);
        this.lstDivisores = [];
        this.lstNumerosPrimos = [];
        this.isLoading = false;
      }
    }
  }
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

.center {
  justify-content: center;
}
</style>
