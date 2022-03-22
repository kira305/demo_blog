<template>
  <div class="login">
    <h2>Dang nhap</h2>
    <form @submit.prevent="login()">
      <div class="form-group">
        <label for="exampleInputEmail1">Email address</label>
        <input v-model="user.email" type="email" class="form-control" :class="{'is-invalid': error.email}">
        <div class="invalid-feedback" v-if="error.email">
          {{error.email[0]}}
        </div>
      </div>
      <div class="form-group">
        <label for="exampleInputPassword1">Password</label>
        <input v-model="user.password" type="password" class="form-control" :class="{'is-invalid': error.password}">
        <div class="invalid-feedback" v-if="error.password">
          {{error.password[0]}}
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
const axios = require('axios').default;
export default {
    data() {
      return {
        user : {
          email:'',
          password:''
        },
        error:{}
      }
    },
    methods: {
      login: function(){
        axios.post('http://localhost:8000/api/login', this.user)
        .then(response => {
          window.localStorage.setItem('token',response.data.token);
          this.$router.push({name: 'home'});
          console.log(response);
        })
        .catch( error => {
          this.error = error.response.data.errors;
          console.log(this.error);
        });
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
