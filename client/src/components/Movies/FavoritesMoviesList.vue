<script setup lang="ts">
import { MoviesStore } from '@/stores/Movies'
import { SettingsStore } from '@/stores/Settings'
import type Movie from '@/types/Movie/Movie'
import MyBtnSearchPush from '../UI/MyBtnSearchPush.vue'
import ListMoviesGrid from './ListMovies/Grid/ListMoviesGrid.vue'

const moviesStore = MoviesStore()

;(async function func() {
  const settingsStore = SettingsStore()
  await moviesStore.getFavoritesMovies(settingsStore.user.favoritesMovies)
})()
const emit = defineEmits<{
  (e: 'look', value: Movie): void
}>()

const look = (movie: Movie) => {
  emit('look', movie)
}
</script>

<template>
  <div v-if="moviesStore.moviesList">
    <ListMoviesGrid :movies="moviesStore.moviesList" @look="look" />
  </div>
  <div v-else class="favorites__movies-noMovies">
    <p>You don't have favorite movies, add at least one</p>
    <MyBtnSearchPush @click="$emit('searchMovies')">Search Movies</MyBtnSearchPush>
  </div>
</template>
