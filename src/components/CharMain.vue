<script setup lang="ts">
import DATA from '../data/data.json'
import type { Character } from '../types/Character';
import { computed, ref } from 'vue';
import CharItem from './CharItem.vue';

const characters = DATA as Character[];
const filterText = ref();
const filteredChars = computed(() => {
    let filter = filterText;
	if (!filter.value) {
        return characters;
    }
    return characters.filter( char => 
        char.name.toLowerCase().includes(filter.value.toLowerCase())
    )
})
</script>

<template>
    <div class="filters">
      <input
        class="search-box"
		placeholder="Filter pokémon..."
		v-model="filterText"
	/>
    </div>
    <div class="list">
        <CharItem v-for="(char, i) in filteredChars" balltype="poke" :character="char" :key="i"/>
    </div>
</template>

<style scoped>
.filters {
    margin-top: 30px;
}
.list {
    margin-top: 30px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
}
</style>