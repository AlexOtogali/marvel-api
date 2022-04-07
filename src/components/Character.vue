<template lang="html">
    <div>
        <div class="flex-container">

            <div class="flex" v-for="char in character">
                <h3>
                    {{char.name}}
                </h3>
                <p>
                    {{char.description}}
                </p>   
            </div>

            <div class="flex2">
                <img :src="url" alt="" class="char-img">
            </div>

        </div>

        

    </div>
</template>

<script>
import axios from 'axios'
import md5 from 'md5'
import { public_key, secret_key } from '../marvel'
export default {
    name:'Character',

    data(){

        return{

            character: [],
            url: '',
            size:'standard_large.jpg',
        }

    },

    mounted(){

        this.getCharacter()
    },

    methods:{
        
        getCharacter: function(){

            const time = Math.floor(Date.now() / 1000 )
                
            const hash = md5 (time + secret_key + public_key )

            var characterId = this.$route.params.id
                    

            axios.get(`http://gateway.marvel.com/v1/public/characters/${characterId}?ts=${time}&apikey=${public_key}&hash=${hash}`)
             .then((result) => {
           
                console.log(result)  
            
                result.data.data.results.forEach((item) => {
                    this.character.push(item)

                    this.url = `${item.thumbnail.path}/${this.size}`
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

    .flex-container{
        
        display: flex;
        align-items: center;
        justify-content: center;
        flex-wrap: wrap;
    }

    .flex{

        flex:50%;
        text-align:center;

    }

    .flex2{

        flex: 50%;
    }

    .char-img{

        width: 20%;
        border-radius: 10px;
    }
</style>