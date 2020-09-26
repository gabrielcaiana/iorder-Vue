<template>
  <div class="container">
    <div class="content image-bg"></div>
    <div class="content">
      <div class="login">
        <img src="../assets/logo.png" alt="iorder logo" />
        <h1>Acesse nosso portal</h1>
        <p>Gerencie seu restaurante de forma fácil e rápida</p>
        <form @submit="submit">
          <input
            :class="{ fieldError: erros.email }"
            v-model="email"
            type="text"
            placeholder="Digite seu email"
          />
          <input
            :class="{ fieldError: erros.senha }"
            v-model="senha"
            type="password"
            placeholder="Digite sua senha"
          />
          <button>Entrar</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      email: "",
      senha: "",
      erros: {
        email: false,
        senha: false,
      },
    };
  },
  methods: {
    async submit(e) {
      e.preventDefault();
      const { data } = await axios.get("http://localhost:3000/users", {
        params: {
          email: this.email,
          senha: this.senha,
        },
      });

      if(data.length > 0) {
        this.$router.push({name: "dashboard"})
      }else {
        alert("Nenhum usuário encontrado")
      }
    },
    checkFields() {
      if(this.email == "") {
        this.erros.email = true
      }else {
        this.erros.email = false
      }

      if(this.senha == "") {
        this.erros.senha = true
      }else {
        this.erros.senha = false
      }
    }
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  display: flex;
  justify-content: space-between;
}

.content {
  width: 50%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.image-bg {
  background-size: cover;
  background-image: url(../assets/bg.png);
  background-repeat: no-repeat;
}

.login {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}

.login img {
  width: 130px;
}

.login h1 {
  font-size: 36px;
  font-weight: 600;
  margin: 32px 0 8px 0;
  color: var(--black);
}

.login p {
  font-size: 18px;
  color: var(--black);
  font-weight: 300;
}

.login form {
  width: 100%;
  display: flex;
  flex-direction: column;
  margin: 32px 0;
}

.login form input {
  width: 420px;
  padding: 20px;
  border-radius: 5px;
  border: 1px solid var(--grayLight);
  margin: 8px 0;
}

.login form input:focus {
  border: 1px solid var(--red-default);
  outline: none;
  border-radius: 5px;
  transition: 0.4s;
}

.login button {
  width: 462px;
  height: 48px;
  padding: 0 20px;
  border: none;
  border-radius: 5px;
  background-color: var(--red-default);
  color: white;
  font-size: 16px;
  transition: 0.5s;
  margin-top: 32px;
}

.login button:hover {
  background-color: var(--red-hover);
}
</style>
