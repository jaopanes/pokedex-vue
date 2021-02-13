<template>
    <div id="app">
       

        <div class="content">
             <Header />
             
            <input type="text" placeholder="Buscar pokemon" v-model="busca" />

            <div class="grid">
                <Pokemon
                    v-for="(poke, index) in computedPokemons"
                    :key="poke.url"
                    :name="poke.name"
                    :url="poke.url"
                    :num="index + 1"
                />
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";
import Header from "./components/Header";

export default {
    name: "App",

    data() {
        return {
            pokemons: [],
            busca: "",
        };
    },

    created: function () {
        axios
            .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
            .then((res) => {
                this.pokemons = res.data.results;
            });
    },

    components: {
        Pokemon,
        Header,
    },

    computed: {
        computedPokemons: function () {
            if (this.busca == "" || this.busca.length < 3) {
                return this.pokemons;
            }
            return this.pokemons.filter((pokemon) =>
                pokemon.name.includes(this.busca.toLowerCase())
            );
        },
    },
};
</script>

<style scoped>
.content {
    width: 100%;
    max-width: 1110px;
    margin: 0 auto;
}
.grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 15px;
}
input {
    width: 100%;
    height: 50px;
    margin-bottom: 30px;
    padding: 0 15px;
}
@media (max-width: 1109px) {
    .grid {
        grid-template-columns: 1fr;
        padding: 30px;
    }
}
</style>
