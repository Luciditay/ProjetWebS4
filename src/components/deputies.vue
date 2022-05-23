<template>
<div id="App">
  <Header/>
  <div id="deputies-gallery" v-if="displayWholeGallery">
    <div id='Options'>
      <SortChoices :deputySortType.sync="deputySortType"/>
      <RandomDeputyButton @update-randomNumber='updateRandomNumber'/>
      <SearchBar @update-search="searchDeputy"/>
    </div>
      <div id="deputyCard"> 
        <Deputy
          v-for="deputy in deputySorted"
          :key="deputy.depute.id"
          :name='deputy.depute.nom' 
          :birthdate="deputy.depute.date_naissance"
          :circonscription='deputy.depute.nom_circo'
          :num_dptmt="deputy.depute.num_deptmt"
          :parliamentary_group="deputy.depute.groupe_sigle"
          :URL_image=" 'https://www.nosdeputes.fr/depute/photo/' + deputy.depute.slug + '/120' "
          :clickable_Image='true'
          :id_depute="deputy.id"/>
          />
      </div>
  </div>

  <div id="randomDeputy" v-if='displayRandomDeputy'>
    <div id='Options'>
      <BackToMenu @update-displayMode='backToMenu'/>
      <RandomDeputyButton @update-randomNumber='updateRandomNumber'/>
      <SearchBar @update-search="searchDeputy"/>
    </div>

    <Deputy v-if="displayRandomDeputy"
      :name="randomDeputyData.nom"
      :birthdate="randomDeputyData.date_naissance"
      :circonscription='randomDeputyData.nom_circo'
      :num_dptmt="randomDeputyData.num_deptmt"
      :parliamentary_group="randomDeputyData.groupe_sigle"
      :previous_job="randomDeputyData.profession"
      :URL_image=" 'https://www.nosdeputes.fr/depute/photo/' + randomDeputyData.slug + '/120' "
      :clickable_Image='false'
      :id_depute="deputy.id"/>
  </div>

  <div id="deputyCard" v-if="displayOneDeputy">
    <div id='Options'>
      <BackToMenu @update-displayMode='backToMenu'/>
      <RandomDeputyButton @update-randomNumber='updateRandomNumber'/>
      <SearchBar @update-search="searchDeputy"/>
    </div>

      <Deputy v-if="deputyData"
        :name="deputyData.nom"
        :birthdate="deputyData.date_naissance"
        :circonscription='deputyData.nom_circo'
        :num_dptmt="deputyData.num_deptmt"
        :parliamentary_group="deputyData.groupe_sigle"
        :previous_job="randomDeputyData.profession"
        :URL_image=" 'https://www.nosdeputes.fr/depute/photo/' + deputyData.slug + '/120'"
        :clickable_Image="false" 
        :id_depute="deputy.id"/>
        <p v-else> Lae député.e recherché.e n'existe pas</p>
  </div> 
  <Footer/>
</div>
</template>

// <script>
import Deputy from './Deputy.vue'
import {nameToSlug} from '@/services/deputiesRepository.js'
import SortChoices from './sortChoices.vue'
import SearchBar from './searchBar.vue'
import RandomDeputyButton from './randomDeputyButton.vue'
import BackToMenu from './backToMenu.vue'
import Header from './header.vue'
import Footer from './footer.vue'

export default {
  name: 'deputies',
  components: {
    Deputy,
    SortChoices,
    SearchBar,
    RandomDeputyButton,
    BackToMenu,
    Header,
    Footer
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
        if (field=="Youngest to Eldest"){
          const comparator = (a, b) => a.depute.date_naissance.localeCompare(b.depute.date_naissance)
          data.sort(comparator).reverse();
        }
        if (field=="Eldest to Youngest"){
          const comparator = (a, b) => a.depute.date_naissance.localeCompare(b.depute.date_naissance)
          data.sort(comparator);
        }
        return data;
    },

    randomDeputyData: function(){
      let data = this.deputiesData.deputes;
      return data[this.randomNumber].depute;
    }
    
  },

   watch: {
        randomNumber: function(newValue){
            localStorage.setItem("randomNumber", newValue);
        },
        
         displayGallery: function(newValue){
        localStorage.setItem("displayWholeGallery", newValue)
      },

       displayDeputy: function(newValue){
        localStorage.setItem("displayOneDeputy", newValue)
      },

      displayRandom: function(newValue){
        localStorage.setItem("displayRandomDeputy", newValue)
      }
    },

    data() {
        return {
            deputyData: null,
            selectedSlug : undefined,
            deputiesData : {deputes:['Aa']},
            search: localStorage.getItem("search") || "",
            deputySortType: localStorage.getItem("deputySortType") || "",
            randomNumber: localStorage.getItem("randomNumber") || 1,
            displayWholeGallery: localStorage.getItem("displayWholeGallery") || true,
            displayOneDeputy: localStorage.getItem("displayOneDeputy") || false,
            displayRandomDeputy: localStorage.getItem("displayRandomDeputy") || false,
            
        }
    },

    created() {
        fetch("https://www.nosdeputes.fr/deputes/enmandat/json")
        .then(res => res.json())
        .then(data => this.deputiesData = data)
        .catch(err => console.log(err.message))
    },

    methods: {

      updateRandomNumber(number){
        this.randomNumber = number;
        this.displayRandomDeputy = true;
        this.displayWholeGallery = false;
        this.displayOneDeputy = false;
      },

      searchDeputy(search) {
        this.search = search
        
        var myHeaders = new Headers();

        var myInit = { method: 'GET',
                  headers: myHeaders,
                  mode: 'cors',
                  cache: 'default' };

        let URL = "https://www.nosdeputes.fr/" + nameToSlug(this.search) + "/json"
        fetch(URL, myInit)
        .then(res => {
          return res.json()})
        .then(data => {
          this.deputyData = data.depute
          })
        .then(this.displayWholeGallery=false)
        .then(this.displayOneDeputy=true)
        .then(this.displayRandomDeputy=false)
        .catch(err => console.log(err.message))
    },

    backToMenu(){
      this.displayWholeGallery=true;
      this.displayOneDeputy=false;
      this.displayRandomDeputy=false;
    }
  }
}
</script>

<style scoped>

  #Options {
    display: flex;
    justify-content: center;
    margin-bottom: 2%;
    position: fixed;
    top:0;
    left:0;
    right:0;
  }

</style>