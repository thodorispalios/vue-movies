<template>
  <div class="container">
    <div class="list" v-for="(post, index) in posts.Search" :key="index">
      <img :src=post.Poster alt="poster-image">
      <h3>{{ post.Title }}</h3>
      <p>{{ post.Year }}</p>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      posts: [],
    }
  },
  methods: {
    async getData() {
      try {
        let response = await fetch('https://www.omdbapi.com/?s=superhero&apikey=f9422ce0') 
        this.posts = await response.json()
      } catch (error) {
        console.log(error)
      }
    },
  },
  created() {
    this.getData();
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
