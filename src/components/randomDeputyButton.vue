<template>
    <div class="randomDeputyButton">
      <button @click="onRandomChanged">Un député aléatoire</button>
    </div>
</template>

<script>

export default {
    name: 'RandomDeputyButton',

    data(){
        return {
            randomNumber: 1
        }
    },

    methods: {
        newRandomNumber: function(){
            return Math.floor(Math.random() * (576));
        },

        onRandomChanged(){
            this.randomNumber = this.newRandomNumber();
            this.$emit('update-randomNumber', this.randomNumber)
        }
    }

    
}

</script>
