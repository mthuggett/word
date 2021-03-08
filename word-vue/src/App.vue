<template>
  <div class="app-container">
    <form @submit.prevent="sendWord">
      <InputText type="text" v-model="text"/>
      <Button class="p-ml-3" type="submit" label="Submit"/>
    </form>
    <div class="p-d-flex p-jc-center p-mt-4" v-if="data">
      <Card style="width: 25em;">
        <template #header>
          <img src="/images/dict.png">
        </template>
        <template #title>
          {{ data.word }}
        </template>
        <template #subtitle>
          {{ data.results.length }} definitions available
        </template>
        <template #content>
          <ul v-for="(result, index) in data.results" :key="index">
            <li style="text-align: left;"> {{ result.definition }} </li>
          </ul>
        </template>
        <template #footer>
          Frequency: {{ data.frequency }}
        </template>
      </Card>
    </div>
    <div class="p-d-flex p-jc-center p-mt-4" v-else>
      {{ error }}
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import * as winston from 'winston'
import BrowserConsole from 'winston-transport-browserconsole'
import 'setimmediate'

import 'primeflex/primeflex.css'
import Card from 'primevue/card'

const level = "debug";
winston.configure({
  transports: [
    new BrowserConsole({
      format: winston.format.simple(),
      level
    })
  ]
});

export default {
  name: 'App',
  components: {
    Card
  },
  data() {
    return {
      data: '',
      error: '',
      text: null
    }
  },
  methods: {
    sendWord() {
      this.getDefinition(this.text)
      this.$toast.add({severity: 'info', summary: 'Hello' + this.text})
    },
    getDefinition(word) {
      this.data = null
      axios.request({
        method: 'GET',
        url: 'https://' + process.env.VUE_APP_WORDAPI_HOST + '/words/' +word,
        headers: {
          'x-rapidapi-key': process.env.VUE_APP_WORDAPI_KEY,
          'x-rapidapi-host': process.env.VUE_APP_WORDAPI_HOST
        }
        })
      .then(response => {
        this.data = response.data
        winston.debug('DEBUG', { 'Word Return Object': response})
      })
      .catch(error => {
        this.error = error.response.data.message
        winston.error('ERROR', { 'Error': error.response.data.message })
      })
    }
  },
  mounted() {
    // axios.request(options)
    // .then(response => {
    //   this.data = response
    //   winston.debug('DEBUG', { 'Word Return Object': response})
    // })
    // .catch(error => {
    //   winston.error('ERROR', { 'Error': error })
    // })
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
