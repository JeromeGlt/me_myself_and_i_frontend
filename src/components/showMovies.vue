<template>
  <div>
    <div class="flexbox" v-if="no_viewed_movie" @click="create_viewed_movie(this.userId, this.movieData.id)">
      <p>{{ movieData.year }} -</p>
      <p>{{ movieData.title }} -</p>
      <p>{{ movieData.director }} -</p>
      <p @click="deleteMovie(movieData.id)">X</p>
      <p>-</p>
      <p @click="modify_movie_section_open">M</p>
      <div v-if="modify_movie_section">
        <label>Acteur</label>
        <select v-model="actor">
          <option>Belmondo</option>
          <option>Clavier</option>
        </select>
        <label>Titre</label>
        <input type="text" v-model="title">
        <label>Réalisateur</label>
        <input type="text" v-model="director">
        <label>Année</label>
        <input type="number" v-model="year">
        <label>Décennie</label>
        <select v-model="decade">
          <option>50</option>
          <option>60</option>
          <option>70</option>
          <option>80</option>
          <option>90</option>
          <option>2000</option>
          <option>10</option>
          <option>20</option>
        </select>
        <button @click="modifyMovie(movieData.id)"></button>
      </div>
    </div>
    <div class="flexbox viewed_text" v-if="viewed_movie" @click="destroy_viewed_movie">
      <p>{{ movieData.year }} -</p>
      <p>{{ movieData.title }} -</p>
      <p>{{ movieData.director }} -</p>
    </div>
  </div>
</template>

<script>

import { mapState } from 'vuex'

export default {
  name: 'showMovies',
  data: () => ({
    modify_movie_section: false,
    no_viewed_movie: true,
    viewed_movie: false,
    actor: '',
    title: '',
    director: '',
    year: null,
    decade: null
  }),
  computed: {
    ...mapState({
      userId: state => state.userId
    }),
  },
  props: [
    'movieData'
  ],
  methods: {
    deleteMovie(id) {

      let actor = this.movieData.actor

      this.$store.dispatch('deleteMovie', { id, actor })
    },
    modify_movie_section_open() {
      this.modify_movie_section = true
    },
    modifyMovie(id) {

      let movieData = {
        title: this.title,
        director: this.director,
        actor: this.actor,
        year: this.year,
        decade: this.decade
      }

      this.$store.dispatch('modifyMovie', { movieData, id })
      this.modify_movie_section = false
    },
    create_viewed_movie(userId, movieId) {

      this.$store.dispatch('create_viewed_movie', { userId, movieId })
      this.no_viewed_movie = false
      this.viewed_movie = true
    }
  }
}
</script>

<style scoped lang="scss">
  .flexbox {
    display: flex;
  }
  .viewed_text {
    text-decoration: underline overline;
  }
</style>
