<!-- TOOD move to individual file -->
<script setup lang="ts">
  import '~/assets/style.css'
  
  const route = useRoute();  
  const TOTAL_POKEMON = 1010;
  var searchVal = ref('');

  // TODO replace 'any' type
  const { data }: {data: any} = await useFetch('https://pokeapi.co/api/v2/pokemon/' + route.params.id );
  const pokemon = data;

  function search(searchVal: string) {
    console.log(searchVal);
  }

  // send user to random pokemon profile
  function randomPoke() {
    let rndId = Math.floor(Math.random() * TOTAL_POKEMON) + 1;
    navigateTo('pokemon-' + rndId);
  }
</script>

<template>
  <!-- TODO move to Header component -->
  <a href="/">Home</a>
  <span class="link" @click="randomPoke()">Random</span>
  <input v-model="searchVal" placeholder="Search by name or id"/> 
  <hr/>

  <!-- TODO handle if (!pokemon) redirect to custom error  -->
  <!-- <span v-if="pokemon">
  </span> -->
  
  <!-- TODO add previous/next buttons -->

  <div class="poke-card center">
    <h1>
      {{ pokemon.name }} (# {{ pokemon.id }})
    </h1>

    <img :src="`${pokemon.sprites.front_default}`" />
    
    <div id="stats-container">
      <div class="stat">
        <!-- converted to match units from Bulbagarden.net -->
        Height: {{ pokemon.height / 10}} m
      </div>
      <div class="stat float-right">
          Weight: {{ pokemon.weight / 10 }} kg
      </div>
    </div>

    <div classname="abilities">
      <h3>
        Abilities:
      </h3>

      <!-- TODO remove trailing comma -->
      <span v-for="ability in pokemon.abilities" v-bind:key="ability">
        {{  ability.ability.name }}, 

        <!-- NOTE these link to the API, could be useful -->
        <!-- <NuxtLink
          :to="`${ability.ability.url}`" target="_blank">
          {{  ability.ability.name }}
        </NuxtLink>,  -->
      </span> 
    </div>
  </div>

</template>