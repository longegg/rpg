<script setup lang="ts">
import DATA from '../data/data.json';
import type { Character } from '../types/Character';
import { ref } from 'vue';
import CharItem from './CharItem.vue';
import CharacterModal from './modals/CharacterModal.vue';

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
let filtersOpen = ref(false);
let sortOrder = ref(1);
let sortActive = ref(false);

const showModal = ref(false);

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

function toggleFilters() {
    filtersOpen.value = !filtersOpen.value;
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

function sortDate(alternate = true) {
    filteredChars.value = filteredChars.value.sort((a, b) => {
        if (sortOrder.value == 1) {
            return a.createdAt.localeCompare(b.createdAt);
        } else {
            return -a.createdAt.localeCompare(b.createdAt);
        }
    });
    sortActive.value = true;
    if (alternate) {
        sortOrder.value = -sortOrder.value;
    }
}

function charSaved() {
    clearSearch();
    clearFilters();
    showModal.value = false;
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
                        <i class="gg-close"></i>
                    </button>
                </div>
                <button
                    class="search-form__magnifier search-form__submit"
                    type="submit"
                    aria-label="search"
                >
                    <i class="gg-search"></i>
                </button>
            </form>
            <div class="add-container">
                <button
                    @click="toggleFilters()"
                    class="filter-toggler"
                    :class="{ open: filtersOpen }"
                >
                    <i class="gg-options"></i>
                    Filtre
                </button>
                <button class="btn btn--narrow" id="show-modal" @click="showModal = true">
                    <i class="gg-add-r"></i>
                </button>

                <CharacterModal
                    :show="showModal"
                    :characters="characters"
                    :races="uniqueRaces"
                    :classes="uniqueClasses"
                    @close="showModal = false"
                    @save="charSaved"
                />
            </div>
        </div>
        <div class="filters" v-if="filtersOpen">
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
                        <th class="sortable" @click="sortDate">
                            <div class="styled-table__flex">
                                Lagret
                                <div class="icons">
                                    <i
                                        :class="{ active: sortActive }"
                                        class="gg-arrow-down sort-icon"
                                    ></i>
                                    <i v-if="!sortActive" class="gg-arrow-up sort-icon"></i>
                                    <i
                                        v-if="sortActive && sortOrder == 1"
                                        class="gg-sort-az sort-icon"
                                    ></i>
                                    <i
                                        v-if="sortActive && sortOrder == -1"
                                        class="gg-sort-za sort-icon"
                                    ></i>
                                </div>
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
.add-container {
    display: flex;
    align-items: center;
    gap: 10px;
}
.icons {
    display: flex;
    align-items: center;
    gap: 2px;
}
.filter-toggler {
    display: flex;
    background: none;
    border: 1px solid #ddd;
    border-radius: 0.35rem;
    align-items: center;
    gap: 12px;
    padding: 10px 15px;
    color: #222;
    font-weight: normal;
    font-size: 12px;
    cursor: pointer;
}
.filter-toggler:hover {
    border-color: #ccc;
}
.filter-toggler.open {
    transition: box-shadow 0.2s cubic-bezier(0.2, 0, 0, 1);
    box-shadow:
        0 0 0 1px rgba(0, 0, 0, 0.5),
        0 0 0 5px rgba(255, 255, 255, 0.7);
}
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
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}
.char-inner {
    padding: 20px 10px 10px;
    display: flex;
    justify-content: space-between;
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
.sort-icon--flipped {
    transform: rotate(180deg) scale(0.5);
}
.clear-button:hover {
    text-decoration: underline;
}
</style>
