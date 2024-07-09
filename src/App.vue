<template>
    <Header />
    <Hero />
    <SearchMatches @MatchesSearchParameters="HandleMatchesSearchParameters"/>
    <Matches :searchParameters="searchParameters" />
    <Footer />
</template>

<script setup>
import Header from './components/Header.vue'
import Hero from './components/Hero.vue'
import SearchMatches from './components/SearchMatches.vue'
import Matches from './components/Matches.vue'
import Footer from './components/Footer.vue'

import { ref } from 'vue';

let searchParameters = ref({team: '', event: '', date: ''});
console.log('Default Search parameters:', searchParameters.value);

const HandleMatchesSearchParameters = (params) => {
    // console.log('Search parameters in the research event:', params);
    searchParameters.value = {...params};
    console.log('Search parameters in the research event:', searchParameters.value);
};
</script>