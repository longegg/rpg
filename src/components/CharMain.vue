<script setup lang="ts">
import DATA from '../data/data.json'
import type { Character } from '../types/Character';
import { computed, ref } from 'vue';
import CharItem from './CharItem.vue';

const characters = DATA as Character[];
const uniqueRaces = [...new Set(characters.map(item => item.race))];
const filterText = ref();
const filterRace = ref();
// const filteredChars = computed(() => {
//     console.log("computing");
    
//     let filter = filterText;
// 	if (!filter.value) {
//         return characters;
//     }
//     return characters.filter( char => 
//         char.name.toLowerCase().includes(filter.value.toLowerCase())
//     )
// })
const filteredChars = computed(() => {
    console.log("computing");
    
    let filter = filterText;
	if (!filter.value) {
        return characters;
    }
    return characters.filter( char => 
        char.name.toLowerCase().includes(filter.value.toLowerCase())
)
});
const onRaceChange = () => {
    console.log(filterRace.value);   
};
</script>

<template>
    <div class="filters">
      <input
        class="search-box"
		placeholder="Søk på navn..."
		v-model="filterText"
	/>
    </div>
    <div class="filters">
        <select v-model="filterRace" @change="onRaceChange()">
            <option selected value="">Velg rase</option>
            <option v-for="(item, key) in uniqueRaces" :key="key" :value="item">
                {{item}}
            </option>
        </select>
    </div>
    <div class="list">
        <table class="styled-table">
            <thead>
                <tr>
                    <th>Navn</th>
                    <th>Rase</th>
                    <th>Klasse</th>
                    <th>Nivå</th>
                    <th>Lagret</th>
                </tr>
            </thead>
            <tbody>
                <CharItem v-for="(char, i) in filteredChars" :character="char" :key="i"/>
        </tbody>
        </table>
    </div>
</template>

<style scoped>
.filters {
    margin-top: 30px;
}
.list {
    margin-top: 30px;
}

table {
    width: 100%;
}
</style>