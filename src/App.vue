<template>
  <div id="app">
    <quote-form
    v-if="!submitted"
    @submit-form="submitForm" />
    <quote-results 
    v-else
    @quote-again="quoteAgain"/>
  </div>
</template>

<script>
import QuoteForm from './components/QuoteForm.vue'
import QuoteResults from './components/QuoteResults.vue'

export default {
  name: 'App',
  components: {
    QuoteForm,
    QuoteResults
  },
  data(){
    return {
      submitted: false
    }
  },
  methods:{
    submitForm(){
      this.submitted = true
    },
    quoteAgain(){
      this.submitted = false
    }
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
