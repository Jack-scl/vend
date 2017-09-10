<template>
  <nav-list :title="$t('favorites.title')" :light="true" :small="true">

    <div v-if="favorites.length == 0" class="text-muted small">{{ $t('favorites.empty') }}</div>

    <nav-list-item
            :label="link.label"
            :icon="link.icon"
            :to="link.to"
            :params="link.params"
            :key="link.id"
            v-for="link in links"/>

  </nav-list>
</template>

<script>
  import { mapGetters } from 'vuex'
  import NavList from './NavList'
  import NavListItem from './NavListItem'

  export default {
    components: {
      NavList,
      NavListItem
    },
    computed: {
      ...mapGetters({
        favorites: 'favorites'
      }),
      links() {
        return this.favorites.map(this.mapFavorite)
      }
    },
    data () {
      return {
        routeMapping: {
          topics: 'topic',
          projects: 'project'
        },
        icons: {
          'projects': 'stack',
          'topics': 'bubbles'
        }
      }
    },
    mounted () {
      this.$store.dispatch('getFavorites')
    },
    methods: {
      mapFavorite(f) {
        return {
          id: f.id,
          icon: this.icons[f.type],
          to: this.routeMapping[f.type],
          params: {id: f.favoritable.id},
          label: f.favoritable.title
        }
      }
    }
  }
</script>
