<template>
  <input class="inputPokemon"
         type="text"
         :placeholder="placeholderString"
         v-model="inputValue"
         @keypress.enter="showPokemon(inputValue)">
  <div class="container">
    <div class="cardContainer">

      <div class="cardPokemon" v-if="pokemonName">
        <div class="imgPokemon">
          <img :src="pokemonImg" alt="Фото покемона грузится">
        </div>


        <div class="info">
          <div class="infoContainer">
            <div class="leftInfo"> {{ pokemonName }}</div>
            <div class="rightInfo"> {{ pokemonType }}</div>
          </div>
        </div>

        <hr>

        <div class="info">
          <div class="infoContainer">
            <div class="leftInfo"> Weight:</div>
            <div class="rightInfo">{{ pokemonWeight }} kg</div>
          </div>
          <div class="infoContainer">
            <div class="leftInfo"> Height:</div>
            <div class="rightInfo">{{ pokemonHeight / 10 }} m</div>
          </div>
        </div>


        <hr>
        <div class="info">
          <div class="infoContainer" v-for="stat in pokemonStats" :key="stat.id">
            <div class="leftInfo">{{ stat.stat.name }}:</div>
            <div class="rightInfo">{{ stat.base_stat }}</div>
          </div>
        </div>

      </div>
      <button class="btn" @click="showPokemon(pokemonId + 1)">></button>
    </div>
    <PokemonsPaginations
        v-on:show-pokemon="showPokemon"
    />
  </div>
</template>

<script>

import PokemonsPaginations from "@/components/PokemonsPaginations";

export default {
  name: "FindPokemon",
  components: {
    PokemonsPaginations
  },
  data() {
    return {
      placeholderString: 'Найди своего покемона',
      inputValue: '',
      pokemonName: '',
      pokemonWeight: '',
      pokemonImg: '',
      pokemonHeight: '',
      pokemonId: '',
      pokemonStats: '',
      pokemonType: '',
      pokemonForms: '',

    }
  },
  methods: {
    async getOnePokemonData(query) {
      let url = `https://pokeapi.co/api/v2/pokemon/${query}/`
      let response = await fetch(url)
      let pokemon = await response.json()
      return pokemon
    },
    async showPokemon(namePokemon) {
      let character = await this.getOnePokemonData(namePokemon)
      this.pokemonName = character.name
      this.pokemonWeight = character.weight
      this.pokemonHeight = character.height
      this.pokemonId = character.id
      this.pokemonStats = character.stats
      this.pokemonType = character.types[0].type.name
      this.pokemonForms = character.forms
      this.pokemonImg = character.sprites.other.dream_world.front_default
    },
  }
}
</script>

<style scoped>
.container {
  margin-top: 20px;
  display: flex;
  justify-content: center;
}

.inputPokemon {
  height: 25px;
  width: 100%;
  font-size: 18px;
  border-radius: 15px;
  padding: 10px;
}

.cardContainer {
  margin: 10px;
}

.cardPokemon {
  border: #2c3e50 solid 10px;
  border-radius: 15px;
  background: white;
}

.imgPokemon {

}

.imgPokemon img {
  width: 300px;
  height: 400px;
}
.info{
  display: flex;
  flex-direction: column;
}
.infoContainer {
  padding: 10px;
  display: inline-flex;
  justify-content: space-between;
  background: #c25205;
  font-family: Arial;
  font-size: 18px;

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