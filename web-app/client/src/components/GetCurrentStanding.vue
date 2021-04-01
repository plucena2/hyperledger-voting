<template>
  <div class="posts">
    <div class="main-posts">
      <div class="subtitle">
        <p>Obtenha o resultado atual</p>
        <p>91: Futebol</p>
        <p>92: Rock</p>
        <p>93: Professora</p>
        <p>94: Copa do Mundo</p>
        <p>95: Vitória Régia</p>
      </div>

      <span v-if="response">
        <b>{{ response }}</b>
      </span>
      <br />
      <button class="btn-confirm" v-on:click="getCurrentStanding()">Confirmar</button>
      <br />

      <div class="chart-wrapper">
        <chart
          :options="chartOptionsBar"
          style="height: 300px; width: 300px; position: absoluted"
        ></chart>
      </div>

      <br />
      <vue-instant-loading-spinner
        id="loader"
        ref="Spinner"
      ></vue-instant-loading-spinner>
    </div>
  </div>
</template>


<script>
import PostsService from "@/services/apiService";
import VueInstantLoadingSpinner from "vue-instant-loading-spinner/src/components/VueInstantLoadingSpinner.vue";
import { Bar } from "vue-chartjs";

export default {
  extends: Bar,
  name: "response",
  data() {
    return {
      response: null,
      chartOptionsBar: {},
    };
  },
  components: {
    VueInstantLoadingSpinner,
  },
  methods: {
    async getCurrentStanding() {
      this.response = null;

      this.runSpinner();

      const apiResponse = await PostsService.getCurrentStanding();
      console.log("%%%%%%%%%%%%%%%%%%%%%%%%%");
      console.log(apiResponse);
      console.log(apiResponse.data[0].Record);
      let currentStanding = [];
      for (let i = 0; i < apiResponse.data.length; i++) {
        currentStanding[i] = apiResponse.data[i].Record.count;
      }
      console.log("curStanding: ");
      console.log(currentStanding);

      this.chartOptionsBar = {
        xAxis: {
          data: ["91", "92", "93", "94", "95"],
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            type: "bar",
            data: currentStanding,
          },
        ],
        color: ["#f8881f"]
      };

      this.hideSpinner();
    },
    async runSpinner() {
      this.$refs.Spinner.show();
    },
    async hideSpinner() {
      this.$refs.Spinner.hide();
    },
  },
};
</script>
