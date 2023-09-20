<script setup lang="ts">

  import '~/assets/style.css'
  
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
  
  <div>
    <h1>Welcome to the Pokedex</h1>
    <span class="subtitle">By Kevin Bagnato</span>
    
    <!-- search bar ~ show only pokemon.filter(search val) -->
    <input v-model="filterVal" placeholder="Filter by name"/> 

    <table>
      <tr>
        <th>Id</th>
        <th>Pic</th>
        <th>Name</th>
      </tr>
      
      <!-- TODO add pokemonList.filterByName(filterVal) -->
      <tr v-for="pokemon in pokemonList" v-bind:key="pokemon">
        <td>
          <!-- id, extracted from url -->
          url
          <!-- {{  pokemon.url.split('/')[6] }} -->
        </td>
        <td>
          sprite
          <!-- {{  pokemon.sprites.front_default }} -->
        </td>
        <td>
          {{  pokemon }}
        </td>
      </tr>
    </table>


    
    
    <!-- table of the top 60 pokemon 
          thumbnail  |  name (href='/<pokemon id'>)
            maybe coordinate row color to pokemon class
      -->
    
    <!-- <NuxtWelcome /> -->
  </div>
</template>
