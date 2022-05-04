<template>
    <div class="search-options">
      <input type="text" v-model="search" name="search" placeholder="Chercher un député">
      <button @click="searchDeputy">Search </button>

      <label for="deputy-sort"> Trier par : </label>
      <select v-model="deputySortType" id="deputy-sort">
		<option value="AZName">Noms de A à Z</option>
      <option value="Party">Parti Politique</option>
      </select>

    </div>
</template>

<script>

export default {
    name: 'GaleryOptions',

    props: {
        search: String,
        deputySortType: String,
    },

    methods: {
        onSearchChanged: function(event){
            this.$emit('update:search', event.target.value)
        },
        onDeputySortTypeChanged(event){
            this.$emit('update:search', event.target.value)
        },

      searchDeputy: function() {
        
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
    }
    }

}
</script>
