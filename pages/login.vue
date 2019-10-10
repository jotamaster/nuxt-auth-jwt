<template>
  <div class="container">
    <h1>Sign in to access the secret page</h1>
    <div>
      <label for="name">
        <input v-model="username" id="name" type="text" >
      </label>
      <label for="password">
        <input v-model="password" id="password" type="password">
      </label>
      <button @click="postLogin">
        login
      </button>
    </div>
  </div>
</template>

<script>
const Cookie = process.client ? require('js-cookie') : undefined

export default {
  middleware: 'notAuthenticated',
  data() {
    return {
      username:'',
      password:''
    }
  },
  methods: {
    postLogin () {
      if(!this.username || !this.password){
        alert('complete los campos')
      }else{
        this.sendCredentials(this.username,this.password)
      }
    },
    async  sendCredentials (username,password){
      const data = await this.$axios.$post('/auth/login',{
        username : this.username,
        password : this.password
      })
      console.log(data)
      

    }

  }
}
</script>
