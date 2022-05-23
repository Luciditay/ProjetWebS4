<template>
    <div class="backToMenu">
      <button @click="onDisplayUpdate">Back to Gallery</button>
    </div>
</template>

<script>

export default {
    name: 'BackToMenu',

   data(){
       return {
           displayMode:{
            displayWholeGallery: true,
            displayOneDeputy: false,
            displayRandomDeputy: false
            }
        }
   },

    // watch: {
    //   displayWholeGallery: function(newValue){
    //     localStorage.setItem("displayWholeGallery", newValue)
    //   },

    //    displayOneDeputy: function(newValue){
    //     localStorage.setItem("displayOneDeputy", newValue)
    //   },

    //   displayRandomDeputy: function(newValue){
    //     localStorage.setItem("displayRandomDeputy", newValue)
    //   }
    // },

    methods: {
        onDisplayUpdate(){
            this.displayMode.displayWholeGallery = true;
            this.displayMode.displayOneDeputy = false;
            this.displayMode.displayRandomDeputy = false;
            this.$emit('update-displayMode', this.displayMode)
        }
    }
}

</script>

<style scoped>
    .backToMenu {
        margin-left: 2%;
        margin-right: 2%;
    }
</style>
