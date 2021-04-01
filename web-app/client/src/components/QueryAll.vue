<template>
  <div class="posts">
    <div class="main-posts">
      <p>Consulta geral</p>
      <p>Veja todos os pares valor-chave</p>
      <button class="btn-confirm" v-on:click="queryAll()">Ver todos</button>

      <header v-if="responseElection">Eleição</header>
      <div id="query-all" v-if="responseElection">
        <p>
          {{
            responseElection[0].Record.election.country +
            " | " +
            responseElection[0].Record.election.name +
            " | " +
            responseElection[0].Record.election.year +
            " | " +
            responseElection[0].Record.election.startDate.substring(0, 10) +
            " | " +
            responseElection[0].Record.election.endDate.substring(0, 10)
          }}
        </p>
      </div>

      <header v-if="responseVotableItem">Candidato</header>
      <div
        id="query-all"
        v-bind:key="responseVotableItem.Key"
        v-for="responseVotableItem in responseVotableItem"
      >
        <p>{{ responseVotableItem }}</p>
      </div>

      <header v-if="responseVoter">Eleitor</header>
      <div
        id="query-all"
        v-bind:key="responseVoter.Key"
        v-for="responseVoter in responseVoter"
      >
        <p>{{ responseVoter }}</p>
      </div>

      <header v-if="responseVoteCount">Contagem de votos</header>
      <div
        id="query-all"
        v-bind:key="responseVoteCount.Key"
        v-for="responseVoteCount in responseVoteCount"
      >
        <p>{{ responseVoteCount }}</p>
      </div>

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
      response: {},
      responseElection: "",
      responseVotableItem: "",
      responseVoteCount: "",
      responseVoter: "",
    };
  },
  components: {
    VueInstantLoadingSpinner,
  },
  methods: {
    async queryAll() {
      this.response = null;
      this.responseElection = null;
      this.responseVoteCount = null;
      this.responseVoter = null;
      this.responseVotableItem = null;

      this.runSpinner();

      const apiResponse = await PostsService.queryAll();
      console.log(apiResponse.data);

      //Voter
      let filterVoter = apiResponse.data.filter(
        (voter) => voter.Record.type === "voter"
      );

      this.responseVoter = filterVoter.map((voter) => {
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

      // Election
      this.responseElection = apiResponse.data.filter(
        (election) => election.Record.type === "ballot"
      );

      //VoteCount
      let filterVoteCount = apiResponse.data.filter(
        (responseVoter) => responseVoter.Record.type === "ballot"
      );

      this.responseVoteCount = filterVoteCount.map((ballot) => {
        return (
          (ballot.Record.election.Record
            ? ballot.Record.election.Record.country + " | "
            : ballot.Record.election.country + " | ") +
          (ballot.Record.election.name
            ? ballot.Record.election.name + " | "
            : ballot.Record.election.Record.name + " | ") +
          (ballot.Record.ballotId + " | ") +
          (ballot.Record.votableItems[0].Record
            ? ballot.Record.votableItems[0].Record.votableId +
              ": " +
              ballot.Record.votableItems[0].Record.description +
              " - " +
              ballot.Record.votableItems[0].Record.count +
              " votos. "
            : ballot.Record.votableItems[0].votableId +
              ": " +
              ballot.Record.votableItems[0].description +
              " - " +
              ballot.Record.votableItems[0].count +
              " votos. ") +
          (ballot.Record.votableItems[1].Record
            ? ballot.Record.votableItems[1].Record.votableId +
              ": " +
              ballot.Record.votableItems[1].Record.description +
              " - " +
              ballot.Record.votableItems[1].Record.count +
              " votos. "
            : ballot.Record.votableItems[1].votableId +
              ": " +
              ballot.Record.votableItems[1].description +
              " - " +
              ballot.Record.votableItems[1].count +
              " votos. ") +
          (ballot.Record.votableItems[2].Record
            ? ballot.Record.votableItems[2].Record.votableId +
              ": " +
              ballot.Record.votableItems[2].Record.description +
              " - " +
              ballot.Record.votableItems[2].Record.count +
              " votos. "
            : ballot.Record.votableItems[2].votableId +
              ": " +
              ballot.Record.votableItems[2].description +
              " - " +
              ballot.Record.votableItems[2].count +
              " votos. ") +
          (ballot.Record.votableItems[3].Record
            ? ballot.Record.votableItems[3].Record.votableId +
              ": " +
              ballot.Record.votableItems[3].Record.description +
              " - " +
              ballot.Record.votableItems[3].Record.count +
              " votos. "
            : ballot.Record.votableItems[3].votableId +
              ": " +
              ballot.Record.votableItems[3].description +
              " - " +
              ballot.Record.votableItems[3].count +
              " votos. ") +
          (ballot.Record.votableItems[4].Record
            ? ballot.Record.votableItems[4].Record.votableId +
              ": " +
              ballot.Record.votableItems[4].Record.description +
              " - " +
              ballot.Record.votableItems[4].Record.count +
              " votos. "
            : ballot.Record.votableItems[4].votableId +
              ": " +
              ballot.Record.votableItems[4].description +
              " - " +
              ballot.Record.votableItems[4].count +
              " votos. ")
        );
      });

      //VotableItem
      let filterVotableItem = apiResponse.data.filter(
        (votableItem) => votableItem.Record.type === "votableItem"
      );

      this.responseVotableItem = filterVotableItem.map((votableItem) => {
        return votableItem.Key + " | " + votableItem.Record.description;
      });

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
