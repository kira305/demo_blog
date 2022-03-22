<template>
  <div class="about">
    <h1>Dashboard</h1>
  </div>
</template>

<script>
const axios = require('axios').default;
export default {
    data() {
      return {
        post : {
          title:'',
          content:''
        },
        error:{}
      }
    },
    mounted() {
      this.checkLoggedIn();
    },
    methods: {
      checkLoggedIn: function(){
        let token = window.localStorage.getItem('token');
        if(token == null){
          this.$router.push({name: 'login'});
        }
        axios.get('http://localhost:8000/api/user',
            {headers: { Authorization: 'Bearer ' + token}}
          )
          .then(function (response) {
            console.log(response);
          })
          .catch( () => {
            this.$router.push({name: 'login'});
        });
      },
    }
}
</script>
