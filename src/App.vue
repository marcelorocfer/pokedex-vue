<template>
    <div id="app">
        <div class="column is-half is-offset-one-quarter">
            <img src="./assets/pokeapi_256.png" alt="Logo">
            <hr>
            <div class="field has-addons">
                <div class="control has-icons-left has-icons-right is-expanded">
                    <label for="buscaPokemon"></label>
                    <input type="text" v-model="busca" id="buscaPokemon" class="input is-info is-rounded" placeholder="Buscar pokemon..." >
                </div>
                <p class="control">
                    <a class="button is-info is-rounded" @click="buscar">Buscar</a>
                </p>
            </div>
            <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
                <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import Pokemon from './components/Pokemon';
    export default {
        name: 'App',
        components: {
            Pokemon
        },
        data: function() {
            return {
                pokemons: [],
                filteredPokemons: [],
                busca: ''
            }
        },
        created: function() {
            axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
                this.pokemons = res.data.results;
                this.filteredPokemons = res.data.results;
            });
        },
        methods: {
            buscar: function() {
                this.filteredPokemons = this.pokemons;
                if(this.busca === '' || this.busca === ' ') {
                    this.filteredPokemons = this.pokemons;
                } else {
                    this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name === this.busca);
                }
            }
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
