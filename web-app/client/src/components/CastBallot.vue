<template>
  <div class="posts">
    <div class="main-posts">
      <p>Registre o seu voto</p>

      <div class="header-container">
        <b>JUSTIÇA<br />ELEITORAL</b>
      </div>
      <div class="container">
        <button class="bnt click" value="1" v-on:click="setVotableItem($event)">
          1
        </button>
        <button class="bnt click" value="2" v-on:click="setVotableItem($event)">
          2
        </button>
        <button class="bnt click" value="3" v-on:click="setVotableItem($event)">
          3
        </button>
        <button class="bnt click" value="4" v-on:click="setVotableItem($event)">
          4
        </button>
        <button class="bnt click" value="5" v-on:click="setVotableItem($event)">
          5
        </button>
        <button class="bnt click" value="6" v-on:click="setVotableItem($event)">
          6
        </button>
        <button class="bnt click" value="7" v-on:click="setVotableItem($event)">
          7
        </button>
        <button class="bnt click" value="8" v-on:click="setVotableItem($event)">
          8
        </button>
        <button class="bnt click" value="9" v-on:click="setVotableItem($event)">
          9
        </button>
        <button class="bnt click" value="0" v-on:click="setVotableItem($event)">
          0
        </button>
      </div>


        <div class="number-ballot">

          <div v-if="showFirstNumber" class="voto1">{{ numbers[0] }}</div>
          <div v-if="picked" class="voto2">{{ numbers[1] }}</div>

          <img v-if="picked==='91'" src="@/assets/img/futebol.png" class="image">
          <img v-if="picked==='92'" src="@/assets/img/rock.png" class="image">
          <img v-if="picked==='93'" src="@/assets/img/professora.png" class="image">
          <img v-if="picked==='94'" src="@/assets/img/copa.png" class="image">
          <img v-if="picked==='95'" src="@/assets/img/vitoriaregia.png" class="image">

        </div>


      <div v-if="name" class="voto">{{ name }}</div>

      <br />
      <br />

      <form v-on:submit="castBallot($event)" style="margin-top: 0">

        <label for="id">Identificação do eleitor</label>
        <br />
        <input
          type="text"
          v-model="input.voterId"
          placeholder="Identificação de eleitor"
        />
        <br />
        <span v-if="response">
          <b>{{ response }}</b>
        </span>

        <div class="btn-voter">
          <button id='btn-reset' type="reset" v-on:click="removeNumber">CORRIGE</button>
          <input id='input-submit' type="submit" value="CONFIRMA" />
        </div>
      </form>

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
      input: {
        voterId: this.$route.query.voterId,
      },
      picked: null,
      response: null,
      image: "",
      name: "",
      showFirstNumber: false,
      numbers: Array(2),
      votableItems: [
        {
          name: "Futebol | Partido dos Esportes",
          value: "91",
        },
        {
          name: "Rock | Partido dos Ritmos Musicais",
          value: "92",
        },
        {
          name: "Professora | Partido das Profissoes",
          value: "93",
        },
        {
          name: "Copa do Mundo | Partido das Festas Populares",
          value: "94",
        },
        {
          name: "Vitória Régia | Partido do Folclore",
          value: "95",
        },
      ],
    };
  },
  components: {
    VueInstantLoadingSpinner,
  },
  methods: {
    async setVotableItem(event) {
      if (this.numbers[0] === undefined) {
        this.numbers[0] = event.target.value;
        this.showFirstNumber = true;
      } else if (this.numbers[1] === undefined) {
        this.numbers[1] = event.target.value;
        this.picked = this.numbers.join("");
        const item = this.votableItems.filter(
          (votableItem) => votableItem.value === this.picked
        )[0];
        this.name = item ? item.name : "";
      }
    },
    async removeNumber() {
      if (this.numbers[1] !== undefined) {
        this.numbers[1] = undefined;
        this.picked = null;
        this.name = "";
      } else if (this.numbers[0] !== undefined) {
        this.numbers[0] = undefined;
        this.showFirstNumber = false;
      }
    },

    async castBallot(event) {
      event.preventDefault();
      await this.runSpinner();

      const electionRes = await PostsService.queryWithQueryString("election");

      let electionId = electionRes.data[0].Key;

      console.log("picked: ", this.picked);
      console.log("voterId: ", this.input.voterId);

      this.response = null;

      //error checking for making sure to vote for a valid party
      if (this.picked === null || this.name === "") {
        console.log("this.picked === null");

        let response = "Por favor, escolha um partido";
        this.response = response;
        await this.hideSpinner();
      } else if (this.input.voterId === undefined) {
        console.log("this.voterId === undefined");

        let response = "É necessário inserir sua identificação";
        this.response = response;
        await this.hideSpinner();
      } else {
        const apiResponse = await PostsService.castBallot(
          electionId,
          this.input.voterId,
          this.picked
        );

        console.log("apiResponse: &&&&&&&&&&&&&&&&&&&&&&&");
        console.log(apiResponse);

        if (apiResponse.data.error) {
          this.response = apiResponse.data.error;
          await this.hideSpinner();
        } else if (apiResponse.data.message) {
          this.response = `Não foi possível encontrar eleitor com identificação ${this.input.voterId}.
          Certifique-se de inserir um número de eleitor válido.`;
          await this.hideSpinner();
        } else {
          this.$router.push("End");
          // let response = `Voto registrado com sucesso para o partido ${this.picked} para eleitor  
          //   com identificação ${apiResponse.data.voterId}. Obrigada por fazer sua parte e
          //   votar!`;
          // console.log(apiResponse.data.voterId);
          // console.log(this.input.voterId);

          // this.response = response;

          console.log("cast ballot finished");
          console.log(this.input);
          
          await this.hideSpinner();
        }
      }
    },
    async runSpinner() {
      this.$refs.Spinner.show();
    },
    async hideSpinner() {
      this.$refs.Spinner.hide();
    },
    async Home() {
      this.$router.push("/");
    },
  },
};
</script>


<style>
.container {
  text-align: center;
  background-color: #414141;
  width: 250px;
  border-radius: 2px;
  padding: 3px 0;
}

.bnt {
  text-align: start;
  background-color: black;
  color: snow;

  height: 30px;
  width: 50px;
  margin: 10px;
  border-radius: 5px;
  cursor: pointer;
}

.bnt:hover {
  background-color: rgb(34, 34, 34);
}

.header-container {
  margin-top: 10px;
  background-color: #b6b5b5;
  color: black;
  width: 250px;
  text-align: center;
  border-radius: 2px;
  padding: 5px 0;
}

.header-container b {
  font-weight: 600;
}

.number-ballot {
  margin-top: 5px;
  display: flex;
  text-align: center;
  background-color: rgba(255, 255, 255, 0.878);
  width: 250px;
  height: 70px;
  padding: 8px 0;
}

.voto1 {
  background-color: white;
  font-size: 40px;
  font-weight: 600;

  width: 25px;
  padding: 5px;
  margin-left: 15px;

  border: 2px;
  border-style: solid;
}

.voto2 {
  background-color: white;
  font-size: 40px;
  font-weight: 600;

  width: 25px;
  padding: 5px;
  margin-right: 60px;

  border: 2px;
  border-style: solid;
}

.voto {
  background-color: rgba(255, 255, 255, 0.878);

  color: black;
  text-align: center;
  font-weight: 600;

  width: 250px;
  padding: 15px 0;
}

.image {
  text-align: end;
  border: 0px;
  border-style: solid;
  width: 60px;
  height: 70px;
  
}
</style>