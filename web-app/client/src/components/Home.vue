<template>
  <div class="posts">
    <div class="main-login">
      <p>Faça a sua votação</p>

      <form v-on:submit="validateVoter">
        <label for="id">Insira sua identificação</label>
        <br />
        <input
          type="text"
          v-model="loginData.voterId"
          placeholder="Identificação de eleitor"
        />
        <br />

        <span v-if="loginReponse">
          <b>{{ loginReponse.data.error }}</b>
        </span>
        <br />
        <br />
        <input type="submit" value="Entrar" />
      </form>
    </div>

    <br />

    <vue-instant-loading-spinner
      id="loader"
      ref="Spinner"
    ></vue-instant-loading-spinner>
  </div>
</template>

<script>
import PostsService from "@/services/apiService";
import VueInstantLoadingSpinner from "vue-instant-loading-spinner/src/components/VueInstantLoadingSpinner.vue";

export default {
  name: "response",
  data() {
    return {
      loginData: {},
      loginReponse: {
        data: "",
      },
    };
  },
  components: {
    VueInstantLoadingSpinner,
  },
  methods: {
    async validateVoter() {
      await this.runSpinner();

      if (!this.loginData.voterId) {
        console.log("!thislogin");
        let response = "Por favor, insira o ID";
        this.loginReponse.data = response;
        await this.hideSpinner();
      } else {
        const apiResponse = await PostsService.validateVoter(
          this.loginData.voterId
        );
        console.log("apiResponse");
        console.log(apiResponse.data);

        if (apiResponse.data.error) {
          console.log('Error: ', apiResponse.data.error);
          this.loginReponse = apiResponse.data.error;
        } else {
          this.$router.push("castBallot");
        }

        console.log(apiResponse);
        this.loginReponse = apiResponse;
        await this.hideSpinner();
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

<style>
.main-login {
  color: #fffbfb;

  border-radius: 5px;
  text-align: start;
  padding-left: 30px;
  align-content: space-between;
}

.main-login p:first-child {
  padding: 0;
  padding-top: 70px;

  font-family: Barlow;
  font-style: normal;
  font-weight: 600;
  font-size: 20px;
  line-height: 24px;

  color: #fffbfb;
}


.main-login label {
  font-family: Barlow;
  font-size: 14px;
  line-height: 25px;

  color: rgba(255, 251, 251, 0.65);
}
</style>
