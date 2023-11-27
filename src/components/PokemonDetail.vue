<template>
    <div>
        <h2>{{ name }}</h2>
        <p>Height:{{ pokemon.height }}</p>
        <p>Weight:{{ pokemon.weight }}</p>
        <img v-bind:src="spriteImgFront"/> &nbsp;
        <img :src="spriteImgBack"/>
        <img :src="shinySpriteImgFront"/>&nbsp;
        <img :src="shinySpriteImgBack"/>
        <div>
            <button v-on:click="savePokemon">Save as a favorite?</button>
        </div>
    </div>
</template>

<script>
import pokemonService from '../services/PokemonService';
export default{
name:"pokemon-detail",
props:{
    id:Number, 
    String,
    name:String,
},
data(){
    return{
        pokemon:{},
        spriteImgFront:"",
        spriteImgBack:"",
        shinySpriteImgFront:"",
        shinySpriteImgBack:"",
    }
},
methods:{
    savePokemon(){
        let pokemonToSave = {
            id:this.id,
            name:this.name,
            url:this.$route.params.url
        };
        pokemonService.saveFavorite(pokemonToSave)

        .then(() => {
                // Handle the response here as needed
                // For example, you can check if the response indicates success or failure
                // and take appropriate actions.
                this.$router.push({ name: 'pokemon' });
            })
            .catch(() => {
                // If there's an error, you can handle it here
                // console.error("Error saving favorite:", error);
                // Optionally, you might want to notify the user about the error
            });
    }

},
created(){
    pokemonService.getPokemonById(this.id).then((response)=>{
            this.pokemon = response.data;
            this.spriteImgFront=response.data.sprites.front_default;
            this.spriteImgBack=response.data.sprites.back_default;
            this.shinySpriteImgFront=response.data.sprites.front_shiny;
            this.shinySpriteImgBack=response.data.sprites.back_shiny;
    })
},
};

</script>

<style>
</style>