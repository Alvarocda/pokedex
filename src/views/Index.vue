<template>
  <div class="row">
    <div class="justify-center">
      <label for="pokemonName">Pokemon name:</label>
      <input
        type="text"
        v-model="pokemonName"
        id="pokemonName"
        @keypress.enter="getPokemon"
      />
      <input type="button" value="Search" @click="getPokemon" />
    </div>
    <div v-if="dataLoaded" class="justify-center">
      <div>
        <img
          :src="pokemonData.sprites.other['official-artwork']['front_default']"
          :alt="pokemonData.name"
          height="400"
        />
      </div>
      <div class="pokemon-name">{{ pokemonData.name }}</div>
      <div class="border data-container">
        <div>Abilities:</div>
        <div v-for="ability in pokemonData.abilities" :key="ability.slot">
          <li>{{ ability.ability.name }}</li>
        </div>
      </div>
      <div class="border data-container">
        <div>Stats:</div>
        <div v-for="stats in pokemonData.stats" :key="stats.stat.name">
          <stat-indicator
            :statName="stats.stat.name"
            :value="stats['base_stat']"
          />
        </div>
        <div>Weight: {{ pokemonData.weight }} Kg</div>
      </div>
    </div>
    <div v-if="error">
      <div>
        No pokemon found with the name <b>{{pokemonName}}.</b>
      </div>
      <div>
        Please, try again with other Pokemon name!
      </div>
      
    </div>
  </div>
</template>

<script>
import axios from "axios";
import StatIndicator from "../components/StatIndicator.vue";
const API_URL = "https://pokeapi.co/api/v2/";

export default {
  components: {
    StatIndicator,
  },
  data() {
    return {
      pokemonName: String(),
      pokemonData: {},
      dataLoaded: false,
      error: false
    };
  },
  methods: {
    getPokemon() {
      axios
        .get(`${API_URL}pokemon/${this.pokemonName}`)
        .then((result) => {
          this.pokemonData = result.data;
          this.dataLoaded = true;
          this.error = false;
        })
        .catch((err) => {
          console.error(err);
          this.dataLoaded = false;
          this.error = true
        });
    },
  },
};
</script>

<style>
.justify-center{
  justify-content: center;
}
.pokemon-name{
  font-size: 35px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  text-align: center;
}
.border {
  border: 1px solid black;
  border-radius: 10px;
}
.data-container {
  margin: 5px;
  padding: 5px;
}
.row {
  display: flex;
}
</style>