<template>
<div class="pager">
  <h1>Movies</h1>
  <div class="buttons">
    <button id="previous-button" v-on:click="currentPage -= 1">Prev</button>
    <button id="next-button" v-on:click="currentPage += 1">Next</button>
  </div>
</div>
  <div class="container">
    <div class="list" v-for="(post, index) in posts.Search" :key="index">
      <div class="list-item" @click="itemToShow =index, toggleShow(), getDetailedData(index)">
        <img v-if="post.Poster!=='N/A'" :src=post.Poster alt="poster-image">
        <img v-else src="./assets/no-image.jpeg" >
        <div class="description">
          <h3>{{ post.Title }}</h3>
          <p>{{ post.Year }}</p>
        </div>
      </div>
      <div v-if="itemToShow == index" class="details" v-show="isVisible">
        <img v-if="post.Poster!=='N/A'" :src=post.Poster alt="poster-image">
        <img v-else src="./assets/no-image.jpeg" >
          <div class="detailed-description">
            <h3 v-if="details.Title">Title : {{details.Title}}</h3>
            <p v-if="details.Plot">Plot : {{details.Plot}}</p>
            <p v-if="details.imdbID">Link : <a :href="'https://imdb.com/title/' + details.imdbID">IMDb</a></p>
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
      currentPage: 1,
      apiKey: 'f9422ce0',
      searchTerm: 'superhero',
      itemToShow: -1,
      isVisible: false,
      details: []
    };
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
    async getDetailedData() {
        try {
          let response = await fetch('https://www.omdbapi.com/?t='+ this.searchTerm + '&apikey=' + this.apiKey) 
          this.details = await response.json()
          console.log(this.details)
        } catch (error) {
          console.log(error)
        }
    },
    toggleShow(){
      this.isVisible = !this.isVisible
    }
  },
  watch: {
    currentPage(value, oldValue) {
      if (value !== oldValue) {
        if (value == 0 || value == 100) {
          this.currentPage = 1;
        }
        this.getData();
      }
    },
  },
  created() {
    this.getData();
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Quicksand:wght@300;400;500;600;700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Quicksand", sans-serif;
}
.container {
  position: absolute;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: stretch;

  .list {
    text-decoration: none;

    .list-item {
      height: 514px;
      width: 322px;
      padding: 10px;
      box-sizing: border-box;
      flex: 1 0 250px;
      margin: 1rem;
      box-shadow: 0 3px 10px rgba(48, 48, 48, 0.1);
      box-shadow:horizontal-offset;

      position: relative;

      .description{
        text-decoration: none;
        position: absolute;
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
  .details{
    background-color: rgba(150, 150, 150, 0.1);
    display: flex;
    padding: 10px;
    width: 50%;

    .detailed-description {
      padding: 10px;

    } 
  }
}
.pager {
    padding: 10px;

    .buttons {
      display: flex;
      justify-content: center;

      button {
        margin: 10px;
        transition: 500ms ease all;
        cursor: pointer;
        margin-top: 24px;
        padding: 12px 24px;
        background-color: #303030;
        color: #fff;
        border-radius: 20px;
        border: none;
        text-transform: uppercase;

        &:focus {
          outline: none;
        }

        &:hover {
          background-color: rgba(48, 48, 48, 0.7);
        }
    }
  }
}
</style>
