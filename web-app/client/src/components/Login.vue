<template>
  <div class="posts">
    <div class="main-register">
      <p>Faça o login</p>

      <form v-on:submit="login($event)">
        <label for="id">Usuário</label><br>
        <input
          type="text"
          v-model="username"
          placeholder="Usuário"
        /><br>
      
        <label for="id">Senha</label><br>
        <input
          type="password"
          v-model="password"
          placeholder="Senha"
        /><br>
       
        <span v-if="registerReponse">
          <b>{{ registerReponse.data }}</b>
        </span>
        <br />
        <br />
        <input type="submit" value="Entrar" />
        <br />
        <br />
      </form>
    </div>
  </div>
</template>

<script>

export default {
  name: "response",
  data() {
    return {
        registerReponse: {
            data: "",
        },
        username: '',
        password: ''
    };
  },
  methods: {
    async login(event) {
      event.preventDefault();
      this.registerReponse.data = '';

      if (
        !this.username ||
        !this.password
      ) {
        console.log("!thislogin");
        let response = "Insira todos os campos";
        this.registerReponse.data = response;
      } 
      else {
        if(this.username === 'tse-IBM-eightbar' && this.password === '!8M-v074ç4O-3!gh7B4r/D1g!t4L/2OZ0'){
            localStorage.setItem('evoteUser', this.username);
            localStorage.setItem('evotePassword', this.password);
            this.$router.push("Register");
        }
        else{
            this.registerReponse.data = 'Usuário ou senha incorretos';
        }
      }
    },
  },
};
</script>

<style>
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

</style>