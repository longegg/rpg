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
    <div class="char-holder">
        <div class="char-inner">
    <div class="search">
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
</div>
</template>

<style scoped>
.filters {
    margin-top: 10px;
}
.char-holder {
    margin-top: 30px;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}
.char-inner {
    padding: 10px;
}
.list {
    margin-top: 20px;
    border-top: 1px solid #ddd;
}

table {
    width: 100%;
}
</style>