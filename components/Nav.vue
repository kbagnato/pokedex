<script setup lang="ts">
  import '~/assets/nav.css';

  const TOTAL_POKEMON = 1010;
  var searchVal = ref('');

  // send user to random pokemon profile
  function randomPoke() {
    let rndId = Math.floor(Math.random() * TOTAL_POKEMON) + 1;
    navigateTo('pokemon-' + rndId);
  }

  // search pokemon by id/name
  function search() {
    console.log(searchVal.value);
    if (searchVal.value == "") {
      window.alert('No search term entered');
    }
    else {
      navigateTo('pokemon-' + searchVal.value);
    }
  }
</script>

<template>
  <div class="nav">

    <span class="link" @click="navigateTo('/')">Home</span>
    
    <span class="link" @click="randomPoke()">Random</span>
    
    <!-- id="searchInput" -->
    <input
      class="link"
      v-model="searchVal"
      @keyup.enter="search()"
      placeholder="Enter name or id"
    /> 
    
    <span class="search-btn" @click="search()">Go</span>
    
    <hr/>
  </div>
</template>