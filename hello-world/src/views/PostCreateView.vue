<template>
  <div class="about">
    <h1>create post</h1>
    <form @submit.prevent="createPost()">
      <div class="form-group">
        <label for="title">Title</label>
        <input v-model="post.title" name="title" type="text" class="form-control" :class="{'is-invalid': error.title}">
        <div class="invalid-feedback" v-if="error.title">
          {{error.title[0]}}
        </div>
      </div>
      <div class="form-group">
        <label for="content">content</label>
        <textarea v-model="post.content" name="content" rows="10" class="form-control" :class="{'is-invalid': error.content}">
        </textarea>
        <div class="invalid-feedback" v-if="error.content">
          {{error.content[0]}}
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
import postService from "../core/postService.js"
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
      postService.checkLoggedIn();
    },
    methods: {
      createPost: function(){
        postService.post('post',this.post)
        .then(response => {
          // this.$router.push({name: 'dashboard'});
          this.post = {title:'', content:''};
          console.log(response);
          alert('create success')
        })
        .catch( error => {
          this.error = error.response.data.errors;
          console.log(this.error);
        });
      }
    }
}
</script>
