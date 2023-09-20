<!-- TOOD move to individual file -->
<script setup lang="ts">
  import '~/assets/style.css'
  
  const route = useRoute();  
  const TOTAL_POKEMON = 1010;
  var searchVal = ref('');

  // TODO replace 'any' type
  const { data }: {data: any} = await useFetch('https://pokeapi.co/api/v2/pokemon/' + route.params.id );
  const pokemon = data;

  function capitalized(input: string) {
    return input.charAt(0).toUpperCase() + input.slice(1);
  }

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
  <a class="link" href="/">Home</a>
  <span class="link" @click="randomPoke()">Random</span>
  <input v-model="searchVal" placeholder="Search by name or id"/> 
  <hr/>
  
  <!-- TODO handle if (!pokemon) redirect to custom error  -->
  <!-- <span v-if="pokemon">
  </span> -->
  
  
  <div class="poke-card center">
    <div class="name">
      {{ capitalized(pokemon.name) }} (#{{ pokemon.id }})
    </div>
    
    <img :src="`${pokemon.sprites.front_default}`" />
    
    
    <div class="stats-container">
      <div class="stat">
        <!-- converted to match units from Bulbagarden.net -->
        <span class="stat-label">Height:</span>
        <span class="stat-value">{{ pokemon.height / 10}} m</span>
      </div>
      <div class="stat">
        <span class="stat-label">Weight:</span>
        <span class="stat-value">{{ pokemon.weight / 10}} kg</span>
      </div>
    </div>
    
    <div classname="abilities">
      <div class="abil-label">Abilities:</div>
      
      <span v-for="ability in pokemon.abilities" v-bind:key="ability" class="abil-value">
        <span v-if="ability != pokemon.abilities[pokemon.abilities.length - 1]">{{  capitalized(ability.ability.name) }}, </span>
        <span v-else>{{  capitalized(ability.ability.name) }}</span>
        
        <!-- NOTE these link to the API, could be useful one day-->
        <!-- <NuxtLink
          :to="`${ability.ability.url}`" target="_blank">
          {{  ability.ability.name }}
        </NuxtLink>,  -->
      </span> 
    </div>
  </div>

  <!-- prev/next buttons -->
  <div class="center">
    <div class="change-poke" @click="navigateTo('pokemon-' + (pokemon.id - 1))">Previous</div>
    <div class="change-poke" @click="navigateTo('pokemon-' + (pokemon.id + 1))">Next</div>
  </div>
</template>