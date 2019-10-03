<template>
  <!-- TODO: 
  
  -
  
  -->
  <div id="app">
    <form action="#" class="search-form" @submit="getFilms">
      <input type="text" placeholder="Название фильма..." v-model="query">
      <input type="submit" value="Искать">
    </form>
    <section v-if="errored" class="info-block">
      <p>Фильм не найден</p>
    </section>
    <section v-else-if="search">
        <div class="info-block" v-if="loading">
          <div class="banter-loader">
            <div class="banter-loader__box"></div>
            <div class="banter-loader__box"></div>
            <div class="banter-loader__box"></div>
            <div class="banter-loader__box"></div>
            <div class="banter-loader__box"></div>
            <div class="banter-loader__box"></div>
            <div class="banter-loader__box"></div>
            <div class="banter-loader__box"></div>
            <div class="banter-loader__box"></div>
          </div>
        </div>
        <div v-else-if="query">
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
import FilmCards from './components/FilmCards.vue'
import axios from 'axios'


export default {
  name: 'app',
  components: {
    FilmCards
  },
  data: function(){
    return{
      search: false,
      loading: true,
      errored: false,
      more: 0,
      page: 1,
      resp: null,
      query: null
    }
  },
  methods: {
    getFilms: function(){
      this.search = true;
      if(this.query == null){
        this.search = false;
      }else{
        axios
        .get(`https://api.themoviedb.org/3/search/movie?api_key=15cbbd361a29be29f6e5bb3b44c6b793&language=ru&query=${this.query}&page=${this.page}&include_adult=false`)
        .then(response => (this.resp = response))
        .finally(() => {
          this.loading = false;
          this.more = this.resp.data.total_pages > 1 ? this.resp.data.total_pages : false;
          this.errored = this.resp.data.total_results === 0 ? true : false;
        });
      }
    },
    getMore: function(){
      this.loading = true;
      this.page = this.page > this.more - 1 ? 1 : ++this.page;
      this.getFilms();
    }
  }
}
</script>