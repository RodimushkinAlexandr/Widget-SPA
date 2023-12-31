<script setup lang="ts">
import type Filters from '@/types/Movie/FiltersMovies'
import FiltersList from './FiltersList.vue'
import { computed } from 'vue'
import { ref } from 'vue'
import FiltersGroup from './FiltersGroup.vue'
import ListMoviesColumn from '../ListMovies/Column/ListMoviesColumn.vue'
import { MoviesStore } from '@/stores/Movies'
import type Movie from '@/types/Movie/Movie'
import LodaderSpinner from '@/components/UI/LoaderSpinner.vue'

const moviesStore = MoviesStore()

const props = defineProps<{
  filters: Filters
}>()

const searchFilter = ref<string>('')
const selectedFilter = ref<string>('genres')

const selected = computed({
  get() {
    return selectedFilter.value
  },
  set(value: string) {
    searchFilter.value = ''
    selectedFilter.value = value
  }
})

const selectedAccurateCategory = async (value: string) => {
  interface FilterMappingsType {
    [key: string]: string
  }
  const FilterMappings: FilterMappingsType = {
    genres: 'genre',
    countries: 'country',
    years: 'year'
  }

  const selectedFilterKey = FilterMappings[selectedFilter.value]
  moviesStore.requestFilters[selectedFilterKey] = value

  filterCategoryProp.value = value
  isShowGroupFilters(false)

  await moviesStore.getMoviesUseFilters()
}

const showGroupFilters = ref<boolean>(true)
const isShowGroupFilters = (value: boolean) => {
  showGroupFilters.value = value
}

const lookMovie = (value: Movie) => {
  moviesStore.lookMovie = value
  moviesStore.showLookMovie = true
}

let filterCategoryProp = ref<string>('')
</script>

<template>
  <div class="filterMain">
    <header class="filterMain__header">
      <h2 class="filterMain__title">CATEGORIES:</h2>
      <FiltersList
        :class="{ disabled: !showGroupFilters }"
        :filters="Object.keys(props.filters)"
        v-model="selected"
        :useFilterStyle="'categories'"
      />
    </header>
    <main class="filterMain__main">
      <FiltersGroup
        v-if="showGroupFilters"
        v-model:search="searchFilter"
        :filters="filters"
        :selected="selected"
        @selected="selectedAccurateCategory"
      />
      <ListMoviesColumn
        v-else-if="moviesStore.moviesList.length"
        :filterCategoryProp="filterCategoryProp"
        :movies="moviesStore.moviesList"
        @backToFilters="isShowGroupFilters"
        @lookMovie="lookMovie"
      />
      <LodaderSpinner v-else />
    </main>
  </div>
</template>

<style lang="scss" scoped>
.disabled {
  pointer-events: none;
}
.filterMain__header {
  max-width: 550px;
  margin: 0 auto;
  display: flex;
  align-items: center;

  border-bottom: 2px solid #9f9fa1;
  margin-bottom: 20px;

  .filterMain__title {
    margin-right: 20px;
  }

  @media (max-width: 500px) {
    flex-direction: column;
    align-items: flex-start;
    font-size: 13px;
    margin-bottom: 10px;
  }
}
</style>
