<script setup lang="ts">

  import '~/assets/style.css'
  import { ref, onMounted } from 'vue';
  import { capitalize } from '~/assets/helpers'
  
  var loading = ref(false);
  var filterVal = ref('');

  // TODO change this 'any' type, ideally implement interface
  // interface Pokemon {
  //   id: number;
  //   name: string;
  // }
  // const { data }: {data: any} = await useFetch('https://pokeapi.co/api/v2/pokemon/?limit=60');
  
  // TODO move to ajax for faster loading; add loading ui
  var pokemonList: any[] = [];
  for (let i = 1; i <= 60; i++) {
    const { data }: {data: any} = await useFetch('https://pokeapi.co/api/v2/pokemon/' + i);
    pokemonList.push(data);
  }

</script>

<template>
  <Nav />
  
  <div class="index center">
    <div class="title">Welcome to the Pokedex</div>
    <span class="subtitle">By Kevin Bagnato</span>
    
    <div v-if="loading">
      <p>Loading...</p>
    </div>
    
    <div v-else>
    <!-- search bar ~ show only pokemon.filter(search val) -->
    <input v-model="filterVal" placeholder="Filter by name"/> 

      <div v-for="pokemon in pokemonList" v-bind:key="pokemon">
        <div class="poke-row" @click="navigateTo('pokemon-' + pokemon.value.id)">
          <img :src="`${pokemon.value.sprites.front_default}`" />
          <div class="poke-name">{{capitalize(pokemon.value.name)}}</div>
        </div>
      </div>

    </div>
  </div>
</template>
