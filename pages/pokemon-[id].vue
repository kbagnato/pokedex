<!-- TOOD move to individual file -->
<script setup lang="ts">
  import '~/assets/style.css'
  import '~/assets/pokeCard.css'
  
  const route = useRoute();  
  
  // TODO replace 'any' type
  const { data }: {data: any} = await useFetch('https://pokeapi.co/api/v2/pokemon/' + route.params.id );
  const pokemon = data;

  function capitalized(input: string) {
    return input.charAt(0).toUpperCase() + input.slice(1);
  }
</script>

<template>
  <Nav />

  <!-- show error if pokemon not found -->
  <div v-if="pokemon == undefined">
    <div class="error-card center">
      <div class='error-label'>Pokemon not found.</div>
    </div>  
  </div>

  <div v-else class="poke-card center">
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
    <div class="change-poke" @click="navigateTo('pokemon-' + (pokemon ? pokemon.id + 1 : 1))">Next</div>
  </div>
</template>