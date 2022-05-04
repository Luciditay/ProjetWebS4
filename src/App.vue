<template>
  <div id="app">
    <deputies/>
  </div>
</template>

// <script>
import deputies from './components/deputies.vue'
//import deputy from './components/deputy.vue'
//import getDeputyDataBySlug from '@/services/deputiesRepository.js'

export default {
  name: 'App',

  components: {
    deputies//, deputy
  },

  data(){
    return{ 
      showGallery: true
    }
  }

    // data() {
    //     return {
    //         deputyData : ["Aa"]
    //     }
    // },


    // methods: {
    //     async retrieveDeputyData(){
    //         this.deputyData = await getDeputyDataBySlug();
    //     }
    // }
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
