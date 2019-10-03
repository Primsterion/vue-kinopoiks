<template>
  <div id="app">
    <form action="#" class="search-form" @submit.prevent="getFilms">
      <input type="text" placeholder="Название фильма..." v-model="query">
      <input type="submit" value="Искать">
    </form>
    <section v-if="error" class="info-block">
      <p>Фильм не найден</p>
    </section>
    <section v-else-if="search">
      <div class="info-block" v-if="loading">
        <Preloader />
      </div>
      <div v-else>
        <FilmCards :posts="this.resp.data.results"></FilmCards>
        <section class="more-block" v-if="more">
          <a href="#" class="btn" @click="getMore">{{this.page === this.more ? "В начало" : "Еще"}}</a>
        </section>
      </div>
    </section>
    <section v-else class="info-block">
      <p>Введите название фильма</p>
    </section>
  </div>
</template>

<script>
  import FilmCards from '@/components/FilmCards'
  import Preloader from '@/components/Preloader'
  import axios from 'axios'


  export default {
    name: 'app',
    components: {
      FilmCards,
      Preloader
    },
    data: function () {
      return {
        search: false, 
        loading: true,
        error: false,
        more: 0,
        page: 1,
        resp: null,
        query: null
      }
    },
    methods: {
      getFilms: function () {
        this.search = true;
        this.loading = true;
        if (this.query == null) {
          this.search = false;
        } else {
          axios
            .get(
              `https://api.themoviedb.org/3/search/movie?api_key=15cbbd361a29be29f6e5bb3b44c6b793&language=ru&query=${this.query}&page=${this.page}&include_adult=false`
            )
            .then(response => (this.resp = response))
            .finally(() => {
              this.loading = false;
              this.more = this.resp.data.total_pages > 1 ? this.resp.data.total_pages : false;
              this.error = this.resp.data.total_results === 0 ? true : false;
            });
        }
      },
      getMore: function () {
        this.loading = true;
        this.page = this.page > this.more - 1 ? 1 : ++this.page;
        this.getFilms();
      }
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    text-decoration: none;
  }

  a:visited {
    color: #fff;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    width: 100%;
    margin: 60px auto;
  }

  body,
  html {
    width: 100%;
    background: #292929;
    scroll-behavior: smooth;
  }

  section {
    margin: 15px 0;
  }

  .search-form {
    width: 100%;
    text-align: center;
  }

  .search-form input[type=text] {
    width: 50%;
    height: 25px;
    border-radius: 5px;
    border: none;
    padding-left: 5px;
    font-size: 1.2rem;
    margin-right: 5px;
  }

  .search-form input[type=submit] {
    width: 10%;
    height: 25px;
    border-radius: 5px;
    border: 1px solid #fff;
    background: transparent;
    color: #fff;
    position: relative;
    bottom: 2px;
    transition: .2s;
  }

  .search-form input[type=submit]:hover {
    background: #fff;
    color: #000;
    cursor: pointer;
  }

  .info-block {
    width: 100%;
    height: 30vh;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
    font-size: 2rem;
  }

  .more-block {
    width: 100%;
    height: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
    font-size: 1.2rem;
    margin-bottom: 10px;
  }

  .btn {
    width: 150px;
    height: 30px;
    border: 1px solid #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: .2s;
  }

  .btn:hover {
    background: #fff;
    color: #000;
    cursor: pointer;
  }

  ::-webkit-scrollbar {
    width: 3px;
    height: 3px;
    background: #000;
  }

  ::-webkit-scrollbar-button {
    display: none;
  }

  ::-webkit-scrollbar-track {
    background-color: #999;
  }

  ::-webkit-scrollbar-track-piece {
    background-color: #000;
  }

  ::-webkit-scrollbar-thumb {
    height: 50px;
    background-color: #fff;
    border-radius: 3px;
  }

  ::-webkit-scrollbar-corner {
    background-color: #000;
  }

  ::-webkit-resizer {
    background-color: #000;
  }

  @media screen and (max-width: 991px) {
    .film-card {
      width: 30%;
    }
  }

  @media screen and (max-width: 768px) {
    .film-card {
      width: 40%;
    }
  }

  @media screen and (max-width: 576px) {
    .film-card {
      width: 65%;
      margin: 15px auto;
    }

    .info-block {
      text-align: center;
    }

    .search-form input[type=text] {
      width: 65%;
    }

    .search-form input[type=submit] {
      width: 20%;
    }
  }

  @media screen and (max-width: 425px) {
    .film-card {
      width: 85%;
    }
  }
</style>