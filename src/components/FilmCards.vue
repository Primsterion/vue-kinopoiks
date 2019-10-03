<template>
  <section class="film-card__wrapper">
    <div class="film-card" v-for="post in posts" @mouseover="getDetails(post.id)" :key="post.id">
      <div class="film-card__back" :style="{background: 'url(' + img + post.backdrop_path + ') no-repeat'} ">
        <div class="film-card-filter">
          <div class="film-card-more">
            <div class="card-more-header">
              <h4 class="card-more-title">{{post.title}}</h4>
              <p class="card-more-rating"><span class="more-rating">{{post.vote_average}}</span></p>
            </div>
            <p class="card-more-date">{{ post.release_date.split('-').reverse().join('.') }} <span>{{time}}</span></p>
            <div class="film-card-desc-block">
              <p>{{post.overview != '' ? post.overview : 'Описание отсутствует'}}</p>
            </div>
            <button class="film-trailer-open-btn">Трейлер</button>
          </div>
        </div>
      </div>
      <div class="film-card__front" :style="{background: 'url(' + img + post.poster_path + ') no-repeat'} ">
        <div class="film-card-info">
          <div class="info-title">
            <p class="film-card__title">{{post.title}}</p>
          </div>
          <div class="info-rating">
            <p class="film-card__rating"><i class="material-icons">star_border</i>{{post.vote_average}}</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  name: 'FilmCards',
  props: {
    posts: null
  },
  data: function(){
    return{
      img: 'https://image.tmdb.org/t/p/original/',
      id: null,
      details: null,
      time: null
    }
  },
  methods:{
    getDetails: function(id){
      axios
        .get('https://api.themoviedb.org/3/movie/' + id + '?api_key=15cbbd361a29be29f6e5bb3b44c6b793&language=ru')
        .then(response => this.details = response.data)
        .finally(() => {
          let hour = Math.floor(this.details.runtime / 60);
          let minutes = this.details.runtime - hour * 60; 
          this.time = hour + ' ч ' + minutes + ' м';
        });

    },
  }
}
</script>
