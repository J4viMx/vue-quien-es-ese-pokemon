<template>
    <div>

        <h1 v-if="!pokemon">Espere por favor...</h1>
        
        <div v-else>

            <h1>¿Quien es este pokémon?</h1>
            <!-- Imagenes -->
            <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
            <!-- Opciones -->
            <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer"/>

            <template v-if="showAnswer">
                <h2 class="fade-in"> {{message}} </h2>
                <button @click="newGame">
                    Nuevo Juego
                </button>
            </template>
        </div>



    </div>
</template>

<script>

import PokemonPicture from '@/components/PokemonPicture'
import PokemonOptions from '@/components/PokemonOptions'

import getPokemonOptions from '@/helpers/getPokemonOptions'


/* console.log(getPokemonOptions()) */

export default {

    components:{
        PokemonPicture,
        PokemonOptions

    },
    data(){
        return{
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()

            const rndInt = Math.floor( Math.random () * 4)
            this.pokemon = this.pokemonArr[ rndInt ]
        },
        checkAnswer(pokemonId) {
            this.showPokemon = true
            this.showAnswer = true

            if( pokemonId === this.pokemon.id ){
                this.message = `Correcto, ${this.pokemon.name}`
            }else{
                this.message = `Ops, era ${this.pokemon.name}`
            }
        },

        newGame(){

            this.showPokemon = false
            this.showAnswer = false
            this.showAnswer = []
            this.mixPokemonArray()
            this.pokemon = null
        }

    },

    mounted(){
        this.mixPokemonArray()
    }

}
</script>