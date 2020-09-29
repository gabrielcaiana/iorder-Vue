<template>
  <div>
    <h1>Seja bem vindo {{ $route.params.name }} {{ $route.params.email }}!</h1>

    <form @submit="changeState">
      <input type="text" v-model="name" placeholder="Digite seu nome" />
      <input type="email" v-model="email" placeholder="Digite seu email" />
      <input
        type="password"
        v-model="password"
        placeholder="Digite sua senha"
      />
      <button>
        {{ checkIsEdit ? "Editar Usuario" : "Adicionar usuario" }}
      </button>
    </form>
    <table class="tabela">
      <thead>
        <tr>
          <th># ID</th>
          <th>Nome</th>
          <th>Email</th>
          <th>Senha</th>
          <th>
            Ações
          </th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(user, index) in users" :key="index">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.password }}</td>
          <td>
            <a href="#" @click="deleteUser($event, user.id)">Apagar</a> |
            <a href="#" @click="loadInfo($event, user)">Editar</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      name: "",
      email: "",
      password: "",
      id: null,
      edit: false,
      users: [],
    };
  },
  mounted() {
    this.loadUsers();
  },
  computed: {
    checkIsEdit() {
      return this.edit;
    },
  },
  methods: {
    async loadUsers() {
      const { data } = await axios.get("http://localhost:3000/users");
      this.users = data;
    },
    async createUser() {
      const { data } = await axios.post("http://localhost:3000/users", {
        name: this.name,
        email: this.email,
        password: this.password,
        id: Math.floor(Math.random(1843, 342432904823)),
      });
      this.name = "";
      this.email = "";
      this.password = "";
      this.loadUsers();
    },
    async deleteUser(e, id) {
      e.preventDefault();
      const { data } = await axios.delete(`http://localhost:3000/users/${id}`);
      this.loadUsers();
    },
    changeState(e) {
      e.preventDefault();
      if (this.checkIsEdit) {
        this.updateUser();
      } else {
        this.createUser();
      }
    },
    loadInfo(e, user) {
      e.preventDefault();
      const { id, name, email, password } = user;
      this.edit = true;
      this.name = name;
      this.email = email;
      this.id = id;
      this.password = password;
    },
    async updateUser() {
      const { data } = await axios.put(
        `http://localhost:3000/users/${this.id}`,
        {
          name: this.name,
          email: this.email,
          password: this.password,
        }
      );
      this.name = "";
      this.email = "";
      this.password = "";
      this.id = null;
      this.edit = false;
      this.loadUsers();
    },
  },
};
</script>

<style lang="scss" scoped>
.tabela {
  width: 100%;
  thead {
    width: 100%;
  }
}
</style>