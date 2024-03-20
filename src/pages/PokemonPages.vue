<template>
        <h1 v-if="!pokemon">Espere por favor...</h1>

        <div v-else>
            <h1>¿Quien es este pókemon?</h1>
            <PokemonPicture 
            :pokemonId="pokemon.id" 
            :showPokemon="showPokemon" 
            />

            <PokemonOptions 
            :pokemons="pokemonsArr" 
            @selection="checkAnswer" 
            />

            <template v-if="showAnswer">
                <h2 class="fade-in"> {{ message }} </h2>
                <button @click="newGame">
                    Nuevo juego
                </button>
            </template>
        </div>
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {
    components: {
        PokemonOptions,
        PokemonPicture,
    },

    data() {
        return {
            pokemonsArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: '',
        }
    },

    methods: {
        async mixPokemonArr () {
            this.pokemonsArr = await getPokemonOptions()
            const rndInt = Math.floor( Math.random() * 4)
            this.pokemon = this.pokemonsArr[ rndInt ]
        },

        checkAnswer ( selectId ) {
            this.showPokemon = true
            this.showAnswer = true

            if ( selectId === this.pokemon.id ) {
                this.message = `Correcto, es ${this.pokemon.name}`
            } else {
                this.message = `Oops!, era ${this.pokemon.name}`
            }
        },

        newGame () {
            this.pokemonsArr = [],
            this.showPokemon = false,
            this.showAnswer = false,
            this.pokemon = null
            this.mixPokemonArr()
        }
    },

    mounted () {
        this.mixPokemonArr()
    }
}
</script>
