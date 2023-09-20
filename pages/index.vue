<script setup lang="ts">
  import '~/assets/style.css'
  import { capitalize } from '~/assets/helpers'

  // TODO change 'any' type
  var pokemonList: any = ref([]);
  var loading = ref(true);
  var filterVal = ref('');
  
  // load pokemon into memory
  function loadPoke() {
    loading.value = true;
    for (let i = 1; i <= 60; i++) {
      const { data }: {data: any} = useFetch('https://pokeapi.co/api/v2/pokemon/' + i);
      pokemonList.value.push(data);
    }  
    loading.value = false;

  }

  onMounted(() => {
    // nextTick used because of bug where data doesn't load initally - see here (https://stackoverflow.com/a/75798077)
    nextTick(async() => {
      loadPoke();
    })
  })

</script>

<template>
  <Nav />
  
  <div class="index center">
    <div class="title">Welcome to the Pokedex</div>
    <span class="subtitle">By Kevin Bagnato</span>
    
    <input id="filter" class="center" v-model="filterVal" placeholder="Filter by name"/> 
    
    <div v-if="loading">
      <p>Loading...</p>
    </div>

    <div v-else>
      <div v-for="pokemon in pokemonList" v-bind:key="pokemon">
        
        <!-- filter by name -->
        <span v-if="pokemon.value.name.toLowerCase().includes(filterVal.toLowerCase())">
          <div class="poke-row" @click="navigateTo('pokemon-' + pokemon.value.id)">
            <img :src="`${pokemon.value.sprites.front_default}`" />
            <div class="poke-name">{{capitalize(pokemon.value.name)}}</div>
          </div>
        </span>
        
      </div>
    </div> 

  </div>
</template>