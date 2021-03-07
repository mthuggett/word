<template>
  <div>
    {{ data }}
  </div>
</template>

<script>
import axios from 'axios'
import * as winston from 'winston'
import BrowserConsole from 'winston-transport-browserconsole'
import 'setimmediate'

const level = "debug";
winston.configure({
  transports: [
    new BrowserConsole({
      format: winston.format.simple(),
      level
    })
  ]
});

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
      winston.debug('DEBUG', { 'Word Return Object': response})
    })
    .catch(error => {
      winston.error('ERROR', { 'Error': error })
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
