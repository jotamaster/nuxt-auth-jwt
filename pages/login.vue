<template>
  <div class="container">
    <h1>Sign in to access the secret page Password@01</h1>
    <div>
      <label for="name">
        <input v-model="username" id="name" type="text" />
      </label>
      <label for="password">
        <input v-model="password" id="password" type="password" />
      </label>
      <button @click="postLogin">login</button>
    </div>
  </div>
</template>

<script>
const Cookie = process.client ? require("js-cookie") : undefined;

export default {
  middleware: "notAuthenticated",
  data() {
    return {
      username: "",
      password: ""
    };
  },
  methods: {
    postLogin() {
      if (!this.username || !this.password) {
        alert("complete los campos");
      } else {
        this.sendCredentials(this.username, this.password);
        
      }
    },

    async sendCredentials(username, password) {
      this.$axios
        .$post("/auth/login", {
          username: this.username,
          password: this.password
        })
        .then(response => {
          console.log(response.token);
          const auth = {
            accessToken: response.token
          };
          this.$store.commit("setAuth", auth); // mutating to store for client rendering
          Cookie.set("auth", auth); // saving token in cookie for server rendering
          this.$router.push("/");
        })
        .catch(error => {
          console.log(error.response);
        });
    }
  }
};
</script>
