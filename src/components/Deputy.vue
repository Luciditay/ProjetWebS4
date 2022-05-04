<template>
    <div class="deputy-info">
        <img id="deputy_picture" v-bind:src="URL_image">
        <form class="description">
            <ul id="name">{{name}}</ul>
            <ul id="birthdate">{{getAge(birthdate)}}  ans</ul>
            <ul id="circo">{{circonscription}} ({{num_dptmt}})</ul>
            <ul id="party">{{parliamentary_group}}</ul>
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
