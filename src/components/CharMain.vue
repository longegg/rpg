<script setup lang="ts">
import DATA from '../data/data.json';
import type { Character } from '../types/Character';
import { ref } from 'vue';
import CharItem from './CharItem.vue';

const characters = (DATA as Character[]).sort((a, b) => {
    return -a.createdAt.localeCompare(b.createdAt);
});
const uniqueRaces = [...new Set(characters.map((item) => item.race))];
const uniqueClasses = [...new Set(characters.map((item) => item.className))];
const filterRace = ref('');
const filterClass = ref('');
const searchInput = ref('');

let filteredChars = ref(characters);
let searchActive = ref(false);
let filtersActive = ref(false);
let sortOrder = ref(1);

function submit() {
    if (!searchInput.value) {
        filteredChars.value = characters;
    } else {
        filteredChars.value = characters.filter((char) =>
            char.name.toLowerCase().includes(searchInput.value.toLowerCase())
        );
    }

    filter();
    searchActive.value = true;
}

function filterChanged() {
    if (searchActive.value && searchInput.value) {
        console.log(`Searched for: `, searchInput.value);
        filteredChars.value = characters.filter((char) =>
            char.name.toLowerCase().includes(searchInput.value.toLowerCase())
        );
    } else {
        filteredChars.value = characters;
    }

    filter();

    filtersActive.value = filterRace.value || filterClass.value ? true : false;
}

function filter() {
    filteredChars.value = filteredChars.value.filter(function (char) {
        let filtered = true;
        if (filterRace.value && filterRace.value.length > 0) {
            filtered = char.race == filterRace.value;
        }

        if (filtered) {
            if (filterClass.value && filterClass.value.length > 0) {
                filtered = char.className == filterClass.value;
            }
        }

        return filtered;
    });
}

function clearSearch() {
    searchInput.value = '';
    searchActive.value = false;
    filteredChars.value = characters;
    filter();
}

function clearFilters() {
    filterRace.value = '';
    filterClass.value = '';
    filtersActive.value = false;
    submit();
}

function sortDate() {
    filteredChars.value = filteredChars.value.sort((a, b) => {
        if (sortOrder.value == 1) {
            return a.createdAt.localeCompare(b.createdAt);
        } else {
            return -a.createdAt.localeCompare(b.createdAt);
        }
    });
    sortOrder.value = -sortOrder.value;
}
</script>

<template>
    <div class="char-holder">
        <div class="char-inner">
            <form class="search-form" ref="form" v-on:submit.prevent="submit">
                <div class="search-form__input-wrapper">
                    <input
                        class="search-form__input"
                        type="text"
                        v-model="searchInput"
                        aria-label="search term"
                        @keydown.esc="clearSearch"
                        placeholder="Søk på navn"
                    />
                    <button
                        type="reset"
                        title="Nullstill søket"
                        @click="clearSearch"
                        :class="{ hidden: searchInput == '' }"
                        class="search-form__reset clear"
                        aria-label="Nullstill søket"
                    >
                        <svg width="20" height="20" viewBox="0 0 20 20">
                            <path
                                d="M10 10l5.09-5.09L10 10l5.09 5.09L10 10zm0 0L4.91 4.91 10 10l-5.09 5.09L10 10z"
                                stroke="currentColor"
                                fill="none"
                                fill-rule="evenodd"
                                stroke-linecap="round"
                                stroke-linejoin="round"
                            ></path>
                        </svg>
                    </button>
                </div>
                <button
                    class="search-form__magnifier search-form__submit"
                    type="submit"
                    aria-label="search"
                >
                    <svg width="20" height="20" class="search-form__label" viewBox="0 0 20 20">
                        <path
                            d="M14.386 14.386l4.0877 4.0877-4.0877-4.0877c-2.9418 2.9419-7.7115 2.9419-10.6533 0-2.9419-2.9418-2.9419-7.7115 0-10.6533 2.9418-2.9419 7.7115-2.9419 10.6533 0 2.9419 2.9418 2.9419 7.7115 0 10.6533z"
                            stroke="currentColor"
                            fill="none"
                            fill-rule="evenodd"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                        ></path>
                    </svg>
                </button>
            </form>
        </div>
        <div class="filters">
            <select class="dropdown" v-model="filterRace" @change="filterChanged()">
                <option value="">Velg rase</option>
                <option v-for="(item, key) in uniqueRaces" :key="key" :value="item">
                    {{ item }}
                </option>
            </select>
            <select class="dropdown" v-model="filterClass" @change="filterChanged()">
                <option value="">Velg klasse</option>
                <option v-for="(item, key) in uniqueClasses" :key="key" :value="item">
                    {{ item }}
                </option>
            </select>
            <button v-if="filtersActive" class="clear-button" @click="clearFilters">
                Nulltill alle
            </button>
        </div>
        <div class="list">
            <table v-if="filteredChars.length" class="styled-table">
                <thead>
                    <tr>
                        <th>Navn</th>
                        <th>Rase</th>
                        <th>Klasse</th>
                        <th>Nivå</th>
                        <th @click="sortDate">
                            <div class="styled-table__flex">
                                Lagret
                                <i class="gg-arrows-v-alt sort-icon"></i>
                                <i class="gg-shape-triangle hidden sort-icon sort-icon--flipped"></i>
                            </div>
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <CharItem v-for="(char, i) in filteredChars" :character="char" :key="i" />
                </tbody>
            </table>
            <div class="empty" v-else>
                Ingen resultater.
                <span v-if="searchActive"
                    >Du har søkt på: <i>{{ searchInput }}</i></span
                >
                <span></span>
            </div>
        </div>
    </div>
</template>

<style scoped>
.filters {
    margin-top: 10px;
    display: flex;
    gap: 10px;
    border-top: 1px solid #ddd;
    padding: 10px 10px 10px 10px;
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
    padding-top: 10px;
    border-top: 1px solid #ddd;
}
table {
    width: 100%;
}
.empty {
    padding: 20px;
    /* display: flex; */
    /* flex-direction: column; */
}
.clear-button {
    outline: none;
    border: none;
    background: none;
    color: #1ea7fd;
    cursor: pointer;
}
.sort-button {
    outline: none;
    border: none;
    background: none;
    display: flex;
    flex-direction: column;
    gap: 0;
    cursor: pointer;
}
.sort-icon {
}
.sort-icon--flipped {
    transform: rotate(180deg) scale(0.5);
}
.clear-button:hover {
    text-decoration: underline;
}
</style>
