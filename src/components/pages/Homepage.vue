<template lang="pug">
  #homepage
    // TODO create Loading Dots Component
    div.loading(:class="loading ? '' : 'loading--end'")
      -for ($i=0;$i<4;$i = $i + 1 )
        .loading__dot
    list-projects(:projects="projects")
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Homepage',
    data () {
      return {
        title: 'Anna Fitzon – Visuelle Kommunikation',
        projects: [],
        loading: true,
        slick: ''
      }
    },

    created () {
      this.getProjects()
      document.title = this.title
    },

    methods: {
      getProjects () {
        axios.get(process.env.API_URL + '/wp-json/wp/v2/projects?per_page=100')
          .then(response => {
            this.loading = false
            this.projects = response.data
          })
          .catch(e => {
            console.log(e)
          })
      }
    },

    components: {
      'list-projects': () => import(/* webpackChunkName: "List Projects" */ '../global/ListProjects.vue')
    }
  }
</script>
