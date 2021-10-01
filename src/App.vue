<template>
  <div class="container">
    <div class="list" v-for="(post, index) in posts.Search" :key="index">
      <div class="list-item" >
        <img :src=post.Poster alt="poster-image">
        <div class="description">
          <h3>{{ post.Title }}</h3>
          <p>{{ post.Year }}</p>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      posts: [],
      apiKey: 'f9422ce0',
      searchTerm: 'superhero',
    }
  },
  methods: {
    async getData() {
      try {
        let response = await fetch('https://www.omdbapi.com/?s='+ this.searchTerm + '&page=' + this.currentPage +'&apikey=' + this.apiKey) 
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
.container {
  position: absolute;
  display: flex;
  flex-wrap: wrap;

  .list {
    text-decoration: none;
    
    .list-item {
      height: 514px;
      width: 322px;
      padding: 10px;
      box-sizing: border-box;
      flex: 1 0 250px;
      margin: 1rem;
      border: 1px solid #000;
      position: relative;

      .description{
        text-decoration: none;
        position:absolute;
        bottom: 0;
        padding: 0 0 5px;
      }

      img {
          width: 300px;
          height: 444px
      }

      &:hover {
        background-color: rgba(48, 48, 48, 0.1);
      }
    }
  }
}
</style>
