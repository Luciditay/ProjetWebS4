<template>
    <div class="sortChoices">
      <label for="deputy-sort"> Trier par : </label>
      <select :value="deputySortType" @input="onDeputySortTypeChanged"  id="deputy-sort">
		<option value="AZName">Noms de A à Z</option>
        <option value="Party">Parti Politique</option>
        <option value="Youngest to Eldest">Plus jeune au plus vieux</option>
        <option value="Eldest to Youngest">Plus vieux au plus jeune</option>
      </select>
    </div>
</template>

<script>

export default {
    name: 'SortChoices',

    props: {
        deputySortType: {type : String}
    },

    watch: {
        deputySortType: function(newDeputySortType){
            localStorage.setItem("deputySortType", newDeputySortType);
        }
    },

    methods: {
        onDeputySortTypeChanged(event){
            this.$emit('update:deputySortType', event.target.value)
        }
    }

}
</script>

<style scoped>
    .sortChoices{
        margin-left: 2%;
        margin-right: 2%;
    }
</style>
