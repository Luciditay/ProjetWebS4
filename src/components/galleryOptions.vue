<template>
    <div class="search-options">
      <input type="text" v-model="searchContent" placeholder="Chercher un député">
      <button @click="onSearchChanged">Search </button>

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
        search: {type : String},
        deputySortType: {type : String}
    },

      data(){
        return {
            searchContent: this.search
        }
    },

    watch:{
        search(newValue){
            this.searchContent = newValue;
        }
    },

    methods: {
        onSearchChanged(){
           // let mySearch = this.search
            this.$emit('update-search', this.searchContent)
        },
        onDeputySortTypeChanged(event){
            this.$emit('update:deputySortType', event.target.value)
        }
    }

}
</script>
