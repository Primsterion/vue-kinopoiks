<template>
    <div class="film-card" @mouseover="getDetails(post.id)" v-bind:key="post.id">
        <div class="film-card__back" :style="{background: 'url(' + img + post.backdrop_path + ') no-repeat'} ">
            <div class="film-card-filter">
                <div class="film-card-more">
                    <div class="card-more-header">
                        <h4 class="card-more-title">{{post.title}}</h4>
                        <p class="card-more-rating"><span class="more-rating">{{post.vote_average}}</span></p>
                    </div>
                    <p class="card-more-date">{{ post.release_date.split('-').reverse().join('.') }}
                        <span>{{time}}</span></p>
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
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'FilmCard',
        props: ['post'],
        data() {
            return {
                img: 'https://image.tmdb.org/t/p/original/',
                details: null,
                time: null
            }
        },
        methods: {
            getDetails: function (id) {
                axios
                    .get('https://api.themoviedb.org/3/movie/' + id +
                        '?api_key=15cbbd361a29be29f6e5bb3b44c6b793&language=ru')
                    .then(response => this.details = response.data)
                    .finally(() => {
                        let hour = Math.floor(this.details.runtime / 60);
                        let minutes = this.details.runtime - hour * 60;
                        this.time = hour + ' ч ' + minutes + ' м';
                    });
            }
        }
    }
</script>

<style scoped>
    .film-card {
        width: 20%;
        height: 400px;
        position: relative;
        perspective: 1000px;
        border-radius: 5px;
        margin: 10px;
    }

    .film-card__front,
    .film-card__back {
        position: absolute;
        width: 100%;
        height: 400px;
        backface-visibility: hidden;
        transition: .4s;
        -webkit-background-size: cover !important;
        background-size: cover !important;
        background-position: center center !important;
        border-radius: 5px;
    }

    .film-card__front {
        color: #fff;
    }

    .film-card:hover .film-card__front {
        transform: rotateY(180deg);
    }

    .film-card:hover .film-card__back {
        transform: rotateY(360deg);
    }

    .film-card__back {
        transform: rotateY(180deg);
    }

    .film-card-info {
        width: 100%;
        height: 400px;
        position: relative;
        border-radius: inherit;
    }

    .info-title,
    .info-rating {
        position: absolute;
        width: 100%;
        min-height: 30px;
        background: #0008;
        display: -webkit-flex;
        display: -moz-flex;
        display: -ms-flex;
        display: -o-flex;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 10px 0;
        text-align: center;
    }

    .info-title {
        border-top-left-radius: inherit;
        border-top-right-radius: inherit;
        top: 0;
    }

    .info-rating {
        border-bottom-left-radius: inherit;
        border-bottom-right-radius: inherit;
        bottom: 0;
        font-size: 1rem;
    }

    .film-card__rating i {
        font-size: 1rem;
        position: relative;
        top: 2px;
        margin-right: 5px;
    }

    .film-card-filter {
        color: #fff;
        background: #0008;
        width: 100%;
        height: 400px;
        border-radius: inherit;
    }

    .film-card-more {
        padding: 5px;
    }

    .film-card-desc-block {
        width: 100%;
        height: 250px;
        overflow-y: scroll;
    }

    .film-trailer-open-btn {
        display: block;
        width: 50%;
        padding: 5px 0;
        text-align: center;
        margin: 25px auto;
        border: none;
        border: 1px solid #fff;
        color: #fff;
        background: #0008;
        transition: .4s;
        border-radius: 6px;
    }

    .film-trailer-open-btn:hover {
        background: #fff;
        color: #000;
        cursor: pointer;
    }

    .film-trailer-videobox {
        position: fixed;
        width: 80%;
        height: 80vh;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }

    .film-trailer__filter {
        width: 100%;
        height: 100vh;
        position: absolute;
        top: 0;
        left: 0;
        background: #0009;
    }

    .film-trailer {
        color: #fff;
        display: none;
    }

    .film-trailer i {
        position: absolute;
        right: 15px;
        top: 10px;
        transition: .4s;
    }

    .film-trailer i:hover {
        cursor: pointer;
        transform: rotate(180deg);
    }

    .card-more-header {
        display: -webkit-flex;
        display: -moz-flex;
        display: -ms-flex;
        display: -o-flex;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .more-rating {
        font-size: 2rem;
    }

    .card-more-date {
        margin-bottom: 10px;
        display: flex;
        justify-content: space-between;
        margin-right: 10px;
    }

    .card-more-title {
        margin-bottom: 5px;
    }
</style>