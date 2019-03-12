<template>
  <div>
    <b-container>
      <b-row>
        <br>
      </b-row>
      <label>
        <b-row>
          <b-col sm="3">Search:</b-col>
          <b-col sm="9">
            <b-form-input v-model="search" type="text" @input="onSearchInput"/>
          </b-col>
        </b-row>
      </label>
      <b-row>
        <ul>
          <li v-for="(ss,index) of filtredByPagination" :key="index">
            <nuxt-link :to="'/starships/'+getIdFromSS(ss)">{{ss.name}}</nuxt-link>
          </li>
        </ul>
      </b-row>
      <b-row>
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          size="lg"
          align="center"
        />
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      starships: [],
      perPage: 5,
      currentPage: 1,
      search: ''
    }
  },
  computed: {
    filtredBySearch() {
      return this.starships.filter(ss => {
        return ss.name.indexOf(this.search) !== -1
      })
    },
    filtredByPagination() {
      return this.filtredBySearch.slice(
        (this.currentPage - 1) * this.perPage,
        this.currentPage * this.perPage
      )
    },
    rows() {
      const rows = this.filtredBySearch.length
      return rows
    }
  },
  async asyncData({ $axios, $route }) {
    let next = 'https://swapi.co/api/starships/?page=1'
    let starships = []
    while (next) {
      const { data } = await $axios.get(next)
      next = data.next
      starships.push(data.results)
    }
    starships = Array.prototype.concat.apply([], starships)
    return { starships }
  },
  created() {
    this.search = this.$route.query.search || ''
  },
  methods: {
    onSearchInput() {
      this.currentPage = 1
      this.$router.replace({ query: { search: this.search } })
    },
    getIdFromSS(ss) {
      const ar = ss.url.split('/')
      const id = ar[ar.length - 2]
      return id
    }
  }
}
</script>

<style  scoped>
label {
  width: 100%;
}
</style>
