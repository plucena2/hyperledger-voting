<template>
  <div class="posts">
    <div class="main-register">
      <p>Cadastro de eleitores</p>

      <form v-on:submit="registerVoter($event)">
        <label for="id">Insira a identificação de eleitor</label><br>
        <input
          type="text"
          v-model="registerData.voterId"
          placeholder="Identificação de eleitor"
        /><br>
      
        <label for="id">Insira a seção</label><br>
        <input
          type="text"
          v-model="registerData.registrarId"
          placeholder="Seção"
        /><br>
       
        <label for="id">Insira o primeiro nome</label><br>
        <input
          type="text"
          v-model="registerData.firstName"
          placeholder="Primeiro nome"
        /><br>

        <label for="id">Insira o sobrenome</label><br>
        <input
          type="text"
          v-model="registerData.lastName"
          placeholder="Sobrenome"
        />
        <br />
        <span v-if="registerReponse">
          <b>{{ registerReponse.data }}</b>
        </span>
        <br />
        <br />
        <input type="submit" value="Registrar" />
        <br />
        <br />
      </form>
    </div>

    <div v-if="registerReponse && qrcodeUrl" class="qrcode-container">
      <qrcode-vue class="qrcode" :value="qrcodeUrl" :size="size" level="H"></qrcode-vue>
    </div>

    <vue-instant-loading-spinner
      id="loader"
      ref="Spinner"
    ></vue-instant-loading-spinner>
  </div>
</template>

<script>
import PostsService from "@/services/apiService";
import VueInstantLoadingSpinner from "vue-instant-loading-spinner/src/components/VueInstantLoadingSpinner.vue";
import QrcodeVue from 'qrcode.vue';

export default {
  name: "response",
  data() {
    return {
      registerData: {},
      registerReponse: {
        data: "",
      },
      qrcodeUrl: '',
      size: 300,
    };
  },
  components: {
    VueInstantLoadingSpinner,
    QrcodeVue,
  },
  methods: {
    async registerVoter(event) {
      event.preventDefault();
      this.qrcodeUrl = '';
      await this.runSpinner();

      if (
        !this.registerData.voterId ||
        !this.registerData.registrarId ||
        !this.registerData.firstName ||
        !this.registerData.lastName
      ) {
        console.log("!thislogin");
        let response = "Insira todos os campos";
        this.registerReponse.data = response;
        await this.hideSpinner();
      } 
      else {
        const apiResponse = await PostsService.registerVoter(
          this.registerData.voterId,
          this.registerData.registrarId,
          this.registerData.firstName,
          this.registerData.lastName
        );
        console.log(apiResponse);
        this.registerReponse = apiResponse;
        this.qrcodeUrl = `${window.location.origin}/#/castballot?voterId=${this.registerData.voterId}`;
      }

      await this.hideSpinner();
    },

    async runSpinner() {
      this.$refs.Spinner.show();
    },
    async hideSpinner() {
      this.$refs.Spinner.hide();
    },
  },
  beforeMount(){
    const username = localStorage.getItem('evoteUser');
    const password = localStorage.getItem('evotePassword');
    if(username !== 'tse-IBM-eightbar' || password !== '!8M-v074ç4O-3!gh7B4r/D1g!t4L/2OZ0'){
      this.$router.push("login");
    }
  },
};
</script>

<style scoped>

.main-register {
  background-color: #0b0727;

  text-align: start;
  margin: 30px;
}

.main-register p:first-child {
  font-family: Barlow;
  font-style: normal;
  font-weight: 600;
  font-size: 20px;
  line-height: 24px;

  color: #fffbfb;

  padding: 0;
  padding-top: 20px;
  margin: 0;
}


.main-register label {
  font-family: Barlow;
  font-size: 14px;
  line-height: 20px;
  color: rgba(255, 251, 251, 0.65);
}

.main-register input[type='text'] {
  margin-bottom: 15px;
}

.qrcode-container {
  display: flex; 
  justify-content: flex-start;
  margin-left: 20px;
  margin-bottom: 20px;
}

.qrcode {
  background: white;
  padding: 15px;
}

</style>