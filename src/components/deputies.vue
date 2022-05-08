<template>
<div id="App">
  <div id="deputies gallery" v-if="displayWholeGallery">
    <div id='galleryOptions'>
      <SearchBar @update-search="searchDeputy"/>
      <SortChoices :deputySortType.sync="deputySortType"/>
      <RandomDeputyButton @update-randomNumber='updateRandomNumber'/>
    </div>

    <Deputy
      v-for="deputy in deputySorted"
      :key="deputy.depute.id"
      :name='deputy.depute.nom' 
      :birthdate="deputy.depute.date_naissance"
      :circonscription='deputy.depute.nom_circo'
      :num_dptmt="deputy.depute.num_deptmt"
      :parliamentary_group="deputy.depute.groupe_sigle"
      :URL_image=" 'https://www.nosdeputes.fr/depute/photo/' + deputy.depute.slug + '/120' "/>
  </div>

  <div id="randomDeputy" v-if='displayRandomDeputy'>
    <BackToMenu @update-displayMode='backToMenu'/>

    <Deputy v-if="displayRandomDeputy"
      :name="randomDeputyData.nom"
      :birthdate="randomDeputyData.date_naissance"
      :circonscription='randomDeputyData.nom_circo'
      :num_dptmt="randomDeputyData.num_deptmt"
      :parliamentary_group="randomDeputyData.groupe_sigle"
      :previous_job="randomDeputyData.profession"
      :URL_image=" 'https://www.nosdeputes.fr/depute/photo/' + randomDeputyData.slug + '/120' "/>
  </div>

  <div id="deputyCard" v-if="displayOneDeputy">
    <div id="ButtonMenu">
      <button @click="backToMenu">Back to main gallery</button>
    </div>
    <div id="deputyInfo">
      <Deputy v-if="deputyData"
        :name="deputyData.nom"
        :birthdate="deputyData.date_naissance"
        :circonscription='deputyData.nom_circo'
        :num_dptmt="deputyData.num_deptmt"
        :parliamentary_group="deputyData.groupe_sigle"
        :previous_job="randomDeputyData.profession"
        :URL_image=" 'https://www.nosdeputes.fr/depute/photo/' + deputyData.slug + '/120' "/>

        <p v-else> Lae député.e recherché.e n'existe pas</p>
    </div>
  </div> 
</div>

  
</template>

// <script>
import Deputy from './Deputy.vue'
import {nameToSlug} from '@/services/deputiesRepository.js'
import SortChoices from './sortChoices.vue'
import SearchBar from './searchBar.vue'
import RandomDeputyButton from './randomDeputyButton.vue'
import BackToMenu from './backToMenu.vue'

export default {
  name: 'deputies',
  components: {
    Deputy,
    SortChoices,
    SearchBar,
    RandomDeputyButton,
    BackToMenu
  },

  computed: {

    deputySorted: function(){
        const field = this.deputySortType;
        let data = this.deputiesData.deputes
        if (field=="AZName"){
          const comparator = (a, b) => a.depute.nom_de_famille.localeCompare(b.depute.nom_de_famille) 
          data.sort(comparator);
          }
        if (field=="Party"){
          const comparator = (a, b) => a.depute.groupe_sigle.localeCompare(b.depute.groupe_sigle) 
          data.sort(comparator);
        }
        return data;
    },

    randomDeputyData: function(){
      let data = this.deputiesData.deputes;
      console.log(data);
      return data[this.randomNumber].depute;
    }
    
  },

    data() {
        return {
            deputyData: null,
            deputiesData : {deputes:['Aa']},
            search: "",
            deputySortType: "",
            randomNumber: 1,
            displayWholeGallery: true,
            displayOneDeputy: false,
            displayRandomDeputy: false
        }
    },

    created() {
        fetch("https://www.nosdeputes.fr/deputes/enmandat/json")
        .then(res => res.json())
        .then(data => this.deputiesData = data)
        .catch(err => console.log(err.message))
    },

    methods: {

        newRandomNumber: function(){
            this.randomNumber = Math.floor(Math.random() * (576));
            this.displayWholeGallery = false;
            this.displayRandomDeputy = true;
        },

      updateRandomNumber(number){
        this.randomNumber = number;
        this.displayRandomDeputy = true;
        this.displayWholeGallery = false;
      },

      searchDeputy(search) {
        this.search = search
        
        var myHeaders = new Headers();

        var myInit = { method: 'GET',
                  headers: myHeaders,
                  mode: 'cors',
                  cache: 'default' };

        let URL = "https://www.nosdeputes.fr/" + nameToSlug(this.search) + "/json"
        console.log(URL)
        fetch(URL, myInit)
        .then(res => {
          console.log(res) 
          return res.json()})
        .then(data => {
          this.deputyData = data.depute
          })
        .then(this.displayWholeGallery=false)
        .then(this.displayOneDeputy=true)
        .catch(err => console.log(err.message))
    },

    backToMenu(displayMode){
      this.displayWholeGallery=displayMode.displayWholeGallery;
      this.displayOneDeputy=displayMode.displayOneDeputy;
      this.displayRandomDeputy=displayMode.displayRandomDeputy;
    }
  }
}
</script>

<style scoped>
  #galleryOptions {
    display: flex;
   justify-content: center;
   margin-bottom: 2%;
  }
</style>