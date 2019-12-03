<template lang="pug">
    .master-container
        FilterView(@gotResponse="displayResponse" @confirmError="displayErrorDialog")
        ResponseView(:response="filterResponse" v-if="!isEmpty")
        md-dialog(:md-active.sync="errorDialog")
            md-dialog-content
                span.md-display-1 Error with your filter.  Please fill in all fields completely.
            md-dialog-actions
                md-button.md-raised.md-accent(@click="errorDialog = false") Close
</template>

<script>

import ResponseView from './components/ResponseView'
import FilterView from './components/FilterView'

export default {
  name: 'app',
  components: {
    FilterView,
    ResponseView
  },
    data() {
      return {
          filterResponse: {},
          errorDialog: false
      }
    },
    computed: {

      isEmpty(){
          for(var prop in this.filterResponse) {
              if(this.filterResponse.hasOwnProperty(prop))
                  return false;
          }
          return true;
      }

    },

    methods: {

        displayErrorDialog(){
            this.errorDialog = true;
        },

      displayResponse(val){

          console.log("response value: ")
          console.log(val)
          this.filterResponse = val;
      },


    }
}
</script>
<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
</style>
