<template>
    <div class="deputy-info">
        <button v-if='clickable_Image'  type='button' @click="loadDeputyCard">
            <img id="deputy_picture" v-bind:src="URL_image">
        </button>
        <img v-else id="deputy_picture" v-bind:src="URL_image">
        <div class="description">
            <p id="name">{{name}}</p>
            <p id="birthdate">{{getAge(birthdate)}} ans</p>
            <p id="circo">Circonscription : {{circonscription}} ({{num_dptmt}})</p>
            <p id="party">{{aliasToGroup(parliamentary_group)}}</p>
            <p id="job"> {{previous_job}}</p>
        </div>
    </div>
</template>

<script>

export default {
    name : 'Deputy',

    props: {
        name : {type : String, required: true},
        birthdate : {type :  String},
        circonscription : {type : String},
        num_dptmt : {type : String},
        parliamentary_group : {type : String},
        previous_job : {type : String},
        URL_image : {type : String},
        clickable_Image : {type : Boolean, default: false},
        id_depute : {type : Number}
    },

    methods: {
        loadDeputyCard(){
            this.$emit('loadClickedDeputy', this.id_depute);
        },



        getAge(dateString){
        let today = new Date();
        let birthdate = new Date(dateString);
        let age = today.getFullYear() - birthdate.getFullYear();
        let m = today.getMonth() - birthdate.getMonth();
        if (m <0 || (m==0 && today.getDate() < birthdate.getDate())) {
            age--;
        }
        return age;
    },

    aliasToGroup(alias){
        switch(alias){
            case 'LREM':
                return 'La République en Marche';
                case 'NI':
                    return 'Non Inscrit';
                case 'LFI':
                    return 'La France Insoumise';
                case 'LR':
                    return 'Les Républicains';
                case 'MODEM':
                    return 'Mouvement Démocrate';
                case 'UDI':
                    return 'Union des démocrates indépendants';
                case 'GDR':
                    return 'Gauche Démocrate et Républicaine';
                case 'AE':
                    return 'Agir Ensemble';
                case 'SOC':
                    return 'Socialistes et Apparentés';
                case 'LT':
                    return 'Libertés et Territoires';

                default:
                    return alias;
            }
        }
    }    
}


</script>

<style scoped>

.deputy-info {
    display: flex; 
    }

img{
width: 150px;
height: 180px;
transition: transform .1s;
padding: 10px ;
max-width: 100%;
}

img:hover{
-ms-transform: scale(1.2); /* IE 9 */
-webkit-transform: scale(1.2); /* Safari 3-8 */
transform: scale(1.2);
}

button {
    background: transparent;
    border: none
}

.p {
    padding : 10px;
    font-size: 4em;
}


</style>
