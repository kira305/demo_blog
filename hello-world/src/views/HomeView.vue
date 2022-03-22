<template>
  <div class="container">
    <div v-for="post in posts" :key="post.id">
      <h2>{{ post.title }}</h2>
      <div>{{ post.content }}</div>
    </div>
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item" v-if="currentPage > 1">
          <a class="page-link" href="#" @click.prevent="currentPage--">Previous</a>
        </li>
        <li class="page-item" v-for="(page,index) in totalPage" :key="index" @click.prevent="currentPage = page" :class="{active: currentPage == page}">
          <a class="page-link" href="#"> {{ page }}</a>
        </li>
        <li class="page-item" v-if="currentPage < totalPage">
          <a class="page-link" href="#" @click.prevent="currentPage++">Next</a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
import postService from "../core/postService.js"

export default {
  data() {
      return {
        posts : [],
        currentPage:1,
        totalPage:0
      }
    },
  mounted() {
    this.getPosts();
  },
  watch: {
    currentPage: function(){
      this.getPosts();
    }
  },
  methods: {
    getPosts: function(){
        postService.get('post?page=' +this.currentPage)
        .then(response => {
          console.log(response);
          this.posts = response.data.data;
          this.currentPage = response.data.current_page;
          this.totalPage = response.data.total;
        });
      }
  }
}
</script>
