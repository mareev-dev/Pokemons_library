<template>
  <div class="pokemonsPagination">
    <button class="btn" @click="showListPokemons">{{ btnShowHidePokemons }}</button>
    <div v-if="notes.length">
      <ListItem
                v-for="note in notes" :key="note.id"
                :note="note"
                v-on:click="$emit('show-pokemon', note.name)"
      />
    </div>
    <button class="btn" @click="prevPokemons" v-if="notes.length"> &#60; </button>
    <button class="btn" @click="nextPokemons" v-if="notes.length"> &#62; </button>
  </div>
</template>

<script>
import ListItem from "@/components/ListItem";
export default {
  name: 'PokemonsPaginations',
  components: {
    ListItem
  },
  data() {
    return {
      offset: 0,
      notes: '',
      btnShowHidePokemons: 'Открыть список покемонов',
    }
  },
  methods: {
     async getAllPokemonData(limit = 2000, offset = 0) {
      let url = `https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset}`
      let response = await fetch(url)
      let pokemons = await response.json()
      return pokemons.results
    },
     async nextPokemons() {
      this.offset = this.offset + 15
      this.notes = await this.getAllPokemonData(15, this.offset)
    },
    async prevPokemons() {
      if (this.offset >= 10) {
        this.offset = this.offset - 15
        this.notes = await this.getAllPokemonData(15, this.offset)
      }
    },
    async showListPokemons() {
       if (!this.notes.length) {
         this.notes = await this.getAllPokemonData(15, 0)
         this.btnShowHidePokemons = 'Скрыть список покемонов'
       }
       else {
         this.notes = ''
         this.btnShowHidePokemons = 'Открыть список покемонов'
       }
    }
  }
}
</script>

<style>
.pokemonsPagination{
  margin: 10px;
}
.btn{
  border: #2c3e50 solid 2px;
  border-radius: 15px;
  background: white;
  font-family: Arial;
  font-size: 18px;
  padding: 10px;
  cursor: pointer;
}
.btn:hover{
  background: #eeeeee;
}
</style>


