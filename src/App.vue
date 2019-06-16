<template>
  <div id="app">
    <v-app dark>
      <router-view/>
    </v-app>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}
</style>
