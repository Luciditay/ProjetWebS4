<template>

    <div id="deputyCard">
        <div id="deputyInformations">
            <deputy 
            :name='deputyData.depute.nom' 
            :birthdate="deputyData.depute.date_naissance"
            :circonscription='deputyData.depute.nom_circo'
            :num_dptmt="deputyData.depute.num_deptmt"
            :parliamentary_group="deputyData.depute.groupe_sigle"
            :URL_image=" 'https://www.nosdeputes.fr/depute/photo/' + nameToSlug(deputyData.depute.nom) + '/120' "/>
        </div>

        <div class="searchBar">
            <input type="text" v-model="searchDeputy" name="search" placeholder="Chercher un député">
            <button @click="searchDeputy">Search </button>
        </div>

        <div class="BackToGallery">
            <button @click="backGallery">Back to the gallery </button>
        </div>
    </div>
</template>

// <script>

import deputy from './components/deputy.vue'
import nameToSlug from '@/services/deputiesRepository.js'

export default {
  name: 'App',

  components: {
    deputy
  },

  data(){
    return{ 
        deputyData : ["Aa"],
        showGallery: true
    }
  },

  created(deputyName){
    let slug = nameToSlug(deputyName);
    let URLDepute = "https://www.nosdeputes.fr/" + slug + "/xml"
    fetch(URLDepute)
    .then(res => res.json())
    .then(data => this.deputyData = data)
    .catch(err => console.log(err.message))
  },

  methods(){
      backGallery = function(){
          this.showGallery=true;
      }
  }
}