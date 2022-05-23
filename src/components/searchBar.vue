<template>
    <div class="search-options">
      <input type="text" v-model="searchContent" placeholder="Chercher un député">
      <button @click="onSearchChanged">Search </button>
    </div>
</template>

<script>

export default {
    name: 'SearchBar',

    props: {
        search: {type: String}
    },

    data(){
        return {
            searchContent: this.search
        }
    },

    watch: {
        search(newValue){
            localStorage.setItem("search", newValue)
            this.searchContent=newValue;
        }
    },

    methods: {
        onSearchChanged(){
            this.$emit('update-search', this.searchContent)
        },
    }
}
</script>

<style scoped>
    .search-options{
        margin-left: 2%;
        margin-right: 2%;
    }
</style>