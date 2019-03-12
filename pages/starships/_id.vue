<template>
  <div>
    <b-container>
      <b-table responsive striped hover :items="[starship]"/>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: null,
      starship: {}
    }
  },
  async asyncData({ $axios, $route, params }) {
    const { id } = params
    const starship = (await $axios.get(`https://swapi.co/api/starships/${id}`))
      .data
    return { id, starship }
  }
}
</script>

<style  scoped>
</style>
