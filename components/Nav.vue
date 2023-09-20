<script setup lang="ts">
  const TOTAL_POKEMON = 1010;
  var searchVal = ref('');

  // send user to random pokemon profile
  function randomPoke() {
    let rndId = Math.floor(Math.random() * TOTAL_POKEMON) + 1;
    navigateTo('pokemon-' + rndId);
  }

  // allow user to search by id/name
  function search() {
    console.log(searchVal.value);
    navigateTo('pokemon-' + searchVal.value);
  }
</script>

<template>
  <a class="link" href="/">Home</a>

  <span class="link" @click="randomPoke()">Random</span>
  
  <input
    id="searchInput"
    v-model="searchVal"
    @keyup.enter="search()"
    placeholder="Search by name or id"
  /> 
  
  <span class="search-btn" @click="search()">Search</span>

  <hr/>
</template>