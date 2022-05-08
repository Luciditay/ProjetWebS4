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
