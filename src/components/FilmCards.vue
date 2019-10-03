<template>
  <section class="film-card__wrapper">
    <FilmCard v-for="post in posts" :post="post" :key="post.id" />
  </section>
</template>

<script>
  import FilmCard from '@/components/FilmCard'

  export default {
    name: 'FilmCards',
    components: {
      FilmCard
    },
    props: {
      posts: null
    }
  }
</script>

<style scoped>
  .film-card__wrapper {
    max-width: 1200px;
    padding: 0 15px;
    flex-wrap: wrap;
    display: flex;
    justify-content: space-between;
    margin: 0 auto;
  }
</style>
