<template>
    <div class="deputy-info">
        <button type='button'>
            <img id="deputy_picture" v-bind:src="URL_image">
        </button>
        <form class="description">
            <ul id="name">{{name}}</ul>
            <ul id="birthdate">{{getAge(birthdate)}}  ans</ul>
            <ul id="circo">Circonscription : {{circonscription}} ({{num_dptmt}})</ul>
            <ul id="party">{{aliasToGroup(parliamentary_group)}}</ul>
            <ul id="job"> {{previous_job}}</ul>
        </form>
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
        URL_image : {type : String}
    },

    methods: {

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

.description {
    padding : 10px;
}

img{
width: 120px;
transition: transform .1s;
padding: 10px ;
}

img:hover{
-ms-transform: scale(1.2); /* IE 9 */
-webkit-transform: scale(1.2); /* Safari 3-8 */
transform: scale(1.2);
}

</style>
