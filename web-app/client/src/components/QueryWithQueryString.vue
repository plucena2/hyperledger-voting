<template>
  <div class="posts">
    <div class="main-posts">
      <p>Consulta por tipo</p>
      <p>Selecione o tipo</p>
      <select v-model="selected">
        <option disabled value>Selecione um</option>
        <option value="ballot">Votos</option>
        <option value="election">Eleição</option>
        <option value="votableItem">Candidato</option>
        <option value="voter">Eleitor</option>
      </select>
      <br />
      <br />
      <button class="btn-confirm" v-on:click="queryByQueryString()">Consultar</button>

      <span v-if="responseError">
        <b> {{ responseError }} </b>
      </span>

      <div v-if="response">
        <p> {{ header }} </p>
        <br />
        <span v-bind:key="object.Key" v-for="object in response">
          <b> {{ object }} </b>
        </span>
      </div>


      <br />
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

export default {
  name: "response",
  data() {
    return {
      selected: {
        data: "",
      },
      response: null,
      header: null,
      responseError: null,
    };
  },
  components: {
    VueInstantLoadingSpinner,
  },
  methods: {
    async queryByQueryString(selected) {
      this.response = null;
      this.header = null;
      this.responseError = null;
      this.runSpinner();

      //check to make sure the user selected something
      if (
        this.selected != "ballot" &&
        this.selected != "election" &&
        this.selected != "voter" &&
        this.selected != "votableItem"
      ) {
        let result = `Por favor, selecione uma opção`;
        this.responseError = result;
        this.hideSpinner();
      } else {
        const apiResponse = await PostsService.queryWithQueryString(
          this.selected
        );
        switch (this.selected) {
          case "votableItem":
            this.header = "Número | Candidato | Quantidade de votos";
            this.response = apiResponse.data.map((votableItem) => {
              return (
                votableItem.Key +
                " | " +
                votableItem.Record.description +
                " | " +
                votableItem.Record.count
              );
            });
            break;
          case "election":
            this.response = apiResponse.data.map((election) => {
              return (
                "Código: " +
                election.Key +
                " | País: " +
                election.Record.country +
                "  |  Eleição: " +
                election.Record.name +
                "  |  Ano: " +
                election.Record.year +
                " | Início: " +
                election.Record.startDate.substring(0, 10) +
                "  | Fim:  " +
                election.Record.endDate.substring(0, 10)
              );
            });
            break;
          case "voter":
            this.header = "Identificação | Nome | Voto";
            this.response = apiResponse.data.map((voter) => {
              return (
                voter.Key +
                " | " +
                voter.Record.firstName +
                " " +
                voter.Record.lastName +
                " | " +
                voter.Record.picked
              );
            });
            break;
            case "ballot":
            this.header = "País | Eleição | ID Votação | Resultados";
            this.response = apiResponse.data.map((ballot) => {
              console.log(ballot)
              return (
                (ballot.Record.election.Record ? ballot.Record.election.Record.country + ' | ' : ballot.Record.election.country + " | ") +
                
                (ballot.Record.election.name ? ballot.Record.election.name + ' | ' : ballot.Record.election.Record.name + " | ") +
                
                (ballot.Record.ballotId + " | ") +


                (ballot.Record.votableItems[0].Record ? ballot.Record.votableItems[0].Record.votableId + ': ' + ballot.Record.votableItems[0].Record.description + ' - ' + ballot.Record.votableItems[0].Record.count + ' votos. '
                : ballot.Record.votableItems[0].votableId + ': ' + ballot.Record.votableItems[0].description + ' - ' + ballot.Record.votableItems[0].count + ' votos. ') +

                (ballot.Record.votableItems[1].Record ? ballot.Record.votableItems[1].Record.votableId + ': ' + ballot.Record.votableItems[1].Record.description + ' - ' + ballot.Record.votableItems[1].Record.count + ' votos. '
                : ballot.Record.votableItems[1].votableId + ': ' + ballot.Record.votableItems[1].description + ' - ' + ballot.Record.votableItems[1].count + ' votos. ') +

                (ballot.Record.votableItems[2].Record ? ballot.Record.votableItems[2].Record.votableId + ': ' + ballot.Record.votableItems[2].Record.description + ' - ' + ballot.Record.votableItems[2].Record.count + ' votos. '
                : ballot.Record.votableItems[2].votableId + ': ' + ballot.Record.votableItems[2].description + ' - ' + ballot.Record.votableItems[2].count + ' votos. ') +

                (ballot.Record.votableItems[3].Record ? ballot.Record.votableItems[3].Record.votableId + ': ' + ballot.Record.votableItems[3].Record.description + ' - ' + ballot.Record.votableItems[3].Record.count + ' votos. '
                : ballot.Record.votableItems[3].votableId + ': ' + ballot.Record.votableItems[3].description + ' - ' + ballot.Record.votableItems[3].count + ' votos. ') +

                (ballot.Record.votableItems[4].Record ? ballot.Record.votableItems[4].Record.votableId + ': ' + ballot.Record.votableItems[4].Record.description + ' - ' + ballot.Record.votableItems[4].Record.count + ' votos. '
                : ballot.Record.votableItems[4].votableId + ': ' + ballot.Record.votableItems[4].description + ' - ' + ballot.Record.votableItems[4].count + ' votos. ')                    
              );
            });

            break;
            
          default:
            this.response = apiResponse.data;
            break;
        }

        console.log("query by object type called");
        this.hideSpinner();
      }
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
