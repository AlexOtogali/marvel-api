<template lang="html">
    <div>
        <div class="card-container">
            <div class="card" v-for="character in characters">
                <h3>{{character.name}}</h3>
                <router-link :to="{ name: 'character', params: { id: character.id} }">
                    <button type="button" name="button" class="btn-view">View</button>
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
import { public_key, secret_key } from '../marvel' 
import axios from 'axios'
import md5 from 'md5'
export default {
    name:'Characters',

    data(){

        return{

            characters: []
        }
    },

    mounted(){
        
        this.getCharacters()
    },

    methods:{
        
        getCharacters: function(){

        const time = Math.floor(Date.now() / 1000 );
            
        const hash = md5 (time + secret_key + public_key );
                    

      axios.get(`http://gateway.marvel.com/v1/public/characters?ts=${time}&apikey=${public_key}&hash=${hash}`)
        .then((result) => {

            result.data.data.results.forEach((item) => {

               console.log(item)   
               
               this.characters.push(item)
            })
        })
        .catch((error) => {

            console.log(error)
        })
     }

    }
}
 

</script>

<style lang="css">
    .card-container{

        margin-left: 600px;
        display: grid;
        grid-template-columns: 200px 200px 200px;
        grid-gap: 30px;        
    }

    .card{
        text-align: center;
        background-color: #333 ;
        min-height: 200px;        

    }

    .card h3{
        color: white;
    }

    .btn-view{
        padding: 10px;
        margin-top: 50px;
        border-radius: 15px;
        width: 120px;
        background-color: transparent ;
        color: white;
        font-size: 12px;
        font-weight: bold;
        cursor: pointer;
    }
</style>