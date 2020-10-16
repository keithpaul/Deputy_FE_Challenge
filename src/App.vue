<template>
  <div id="app">
    <BlogMainPage/>
  </div>
</template>

<script>
import BlogMainPage from './components/BlogMainPage.vue'

export default {
  name: 'App',
  components: {
    BlogMainPage
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=PT+Sans:wght@400;700&display=swap');
*{
  margin: 0;
  padding: 0;
  list-style-type: none;
  font-family: 'PT Sans', sans-serif;
  font-weight: 400;
}
</style>
