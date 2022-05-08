<template>
    <div class="sortChoices">
      <label for="deputy-sort"> Trier par : </label>
      <select :value="deputySortType" @input="onDeputySortTypeChanged"  id="deputy-sort">
		<option value="AZName">Noms de A à Z</option>
        <option value="Party">Parti Politique</option>
      </select>
    </div>
</template>

<script>

export default {
    name: 'SortChoices',

    props: {
        deputySortType: {type : String}
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
        margin-right: 3%;
    }
</style>
