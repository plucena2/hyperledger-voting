<template>
  <div class="posts">
    <div class="main-posts">
    <p>Consulta por chave</p>
    <p>Insira uma palavra-chave</p>
    <form v-on:submit="queryByKey">
      <input type="text" v-model="input.key" placeholder="Insira uma palavra-chave">
      <br>
      <br>

      <span v-if="input && input.type === 'text'">
        <b>{{ input.data }}</b>
      </span>
      
      <input type="submit" value="Consultar">
      <br>

      <div v-if="input && input.type === 'json'">
        <span v-bind:key="index" v-for="(information, index) in input.data">
          <b>{{ information }}</b>
        </span>
      </div>      
    </form>

    <br>
      <vue-instant-loading-spinner id='loader' ref="Spinner"></vue-instant-loading-spinner>
    </div>
  </div>
</template>

<script>
import PostsService from "@/services/apiService";
import VueInstantLoadingSpinner from 'vue-instant-loading-spinner/src/components/VueInstantLoadingSpinner.vue'

export default {
  name: "response",
  data() {
    return {
     input: {
        data: "",
        type: ''
      }
    };
  },
  name: 'app',
  components: {
    VueInstantLoadingSpinner
  },
  methods: {
    async queryByKey() {
      this.runSpinner();
      console.log('this.input: ', this.input);

      if (!this.input.key) {
        let response = 'Por favor, digite uma palavra-chave';
        this.input.data = response;
        this.input.type = 'text';
        this.hideSpinner();
      } else {
        this.runSpinner();
        const apiResponse = await PostsService.queryByKey(this.input.key);
        console.log(apiResponse.data);
        if(apiResponse.headers['content-type'].includes('json')){
          this.input.type = 'json';
          switch (apiResponse.data.type) {
            case 'votableItem':
              this.input.data = [
                `Número: ${apiResponse.data.votableId}`,
                `Candidato: ${apiResponse.data.description}`,
                `Quantidade de votos: ${apiResponse.data.count}`
              ];
              break;
            case 'election':
              this.input.data = [
                `Código: ${apiResponse.data.electionId}`,
                `País: ${apiResponse.data.country}`,
                `Eleição: ${apiResponse.data.name}`,
                `Ano: ${apiResponse.data.year}`,
                `Início: ${apiResponse.data.startDate.substring(0, 10)}`,
                `Fim: ${apiResponse.data.endDate.substring(0, 10)}`,
              ];
              break;
            case 'voter':
              this.input.data = [
                `Identificação: ${apiResponse.data.voterId}`,
                `Nome: ${apiResponse.data.firstName} ${apiResponse.data.lastName}`,
                `Voto: ${apiResponse.data.picked}`
              ];
              break;
            case 'ballot':
              this.input.data = [
                'País: ' + (apiResponse.data.election ? apiResponse.data.election.country : apiResponse.data.election.country),
                
                'Eleição: ' + (apiResponse.data.election.name ? apiResponse.data.election.name : apiResponse.data.election.name),
                
                'ID Votação: ' + (apiResponse.data.ballotId),


                'Resultados: ' + (apiResponse.data.votableItems[0] ? apiResponse.data.votableItems[0].votableId + ': ' + apiResponse.data.votableItems[0].description + ' - ' + apiResponse.data.votableItems[0].count + ' votos. '
                : apiResponse.data.votableItems[0].votableId + ': ' + apiResponse.data.votableItems[0].description + ' - ' + apiResponse.data.votableItems[0].count + ' votos. ') +

                (apiResponse.data.votableItems[1] ? apiResponse.data.votableItems[1].votableId + ': ' + apiResponse.data.votableItems[1].description + ' - ' + apiResponse.data.votableItems[1].count + ' votos. '
                : apiResponse.data.votableItems[1].votableId + ': ' + apiResponse.data.votableItems[1].description + ' - ' + apiResponse.data.votableItems[1].count + ' votos. ') +

                (apiResponse.data.votableItems[2] ? apiResponse.data.votableItems[2].votableId + ': ' + apiResponse.data.votableItems[2].description + ' - ' + apiResponse.data.votableItems[2].count + ' votos. '
                : apiResponse.data.votableItems[2].votableId + ': ' + apiResponse.data.votableItems[2].description + ' - ' + apiResponse.data.votableItems[2].count + ' votos. ') +

                (apiResponse.data.votableItems[3] ? apiResponse.data.votableItems[3].votableId + ': ' + apiResponse.data.votableItems[3].description + ' - ' + apiResponse.data.votableItems[3].count + ' votos. '
                : apiResponse.data.votableItems[3].votableId + ': ' + apiResponse.data.votableItems[3].description + ' - ' + apiResponse.data.votableItems[3].count + ' votos. ') +

                (apiResponse.data.votableItems[4] ? apiResponse.data.votableItems[4].votableId + ': ' + apiResponse.data.votableItems[4].description + ' - ' + apiResponse.data.votableItems[4].count + ' votos. '
                : apiResponse.data.votableItems[4].votableId + ': ' + apiResponse.data.votableItems[4].description + ' - ' + apiResponse.data.votableItems[4].count + ' votos. ')
              ]
              break;            
            default:
              break;
          }
        }
        else{
          this.input = apiResponse;
          this.input.type = 'text';
        }
        this.hideSpinner();
      }
    },
    async runSpinner() {
      this.$refs.Spinner.show();
    },
    async hideSpinner() {
      this.$refs.Spinner.hide();
    }
  }
};
</script>
