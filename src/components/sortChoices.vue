<template>
    <div class="sortChoices">
      <label for="deputy-sort"> Trier par : </label>
      <select v-model="deputySortType_tmp" id="deputy-sort">
		<option value="AZName">Noms de A à Z</option>
        <option value="Party">Parti Politique</option>
      </select>
    </div>
</template>

<script>

export default {
    name: 'SortChoices',

    props: {
        search: {type : String},
        deputySortType: {type : String}
    },

    data(){
        return {
            searchContent: this.search,
            deputySortType_tmp: this.deputySortType
        }
    },

    watch:{
        search(newValue){
            this.searchContent = newValue;
        },
        deputySortType(newValue){
            this.deputySortType_tmp = newValue;
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
