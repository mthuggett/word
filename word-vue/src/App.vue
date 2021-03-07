<template>
  <div>
    {{ data }}
  </div>
</template>

<script>
import axios from 'axios'

const options = {
  method: 'GET',
  url: 'https://' + process.env.VUE_APP_WORDAPI_HOST + '/words/example',
  headers: {
    'x-rapidapi-key': process.env.VUE_APP_WORDAPI_KEY,
    'x-rapidapi-host': process.env.VUE_APP_WORDAPI_HOST
  }
}; 

export default {
  name: 'App',
  components: {
    
  },
  data() {
    return {
      data: ''
    }
  },
  mounted() {
    axios.request(options)
    .then(response => {
      this.data = response
    })
    .catch(error => {
      this.data = error
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
