<script setup lang="ts">
import { computed, reactive, ref, toRefs } from 'vue';
import modal from '../modals/ModalBox.vue';
import type { Character } from '../../types/Character';

const props = defineProps<{
    show: Boolean;
    characters: Character[];
    races: any;
    classes: any;
}>();

const { characters } = toRefs(props);
const { races } = toRefs(props);
const { classes } = toRefs(props);

const emit = defineEmits<{
    (e: 'close'): void;
    (e: 'save', id: number): void;
}>();

type FormData = {
    name: string;
};

const formData = reactive({} as FormData);

const nameInput = ref('');
const selectedRace = ref(races.value[0]);
const selectedClass = ref(classes.value[0]);
const levelInput = ref(1);

const isValidName = computed(() => formData.name.length > 0);

const close = () => emit('close');

function save() {
    console.log('save');
    // Return new list with added item and close modal.

    characters.value.push({
        name: nameInput.value,
        race: selectedRace.value,
        className: selectedClass.value,
        level: levelInput.value,
        createdAt: new Date().toISOString()
    } as Character);

    emit('close');
}
</script>

<template>
    <div>
        <Teleport to="body">
            <modal :show="show">
                <template #header>
                    <h2>Legg til ny</h2>
                </template>
                <template #body>
                    <form class="form" ref="form" v-on:submit.prevent="save">
                        <div>
                            <label class="form__label" for="name">Navn:</label>
                            <input
                                id="name"
                                class="form__input"
                                type="text"
                                v-model="nameInput"
                                aria-label="search term"
                                placeholder=""
                            />
                        </div>
                        <div class="form__group">
                            <label class="form__label" for="race">Rase:</label>
                            <select id="race" class="dropdown" v-model="selectedRace">
                                <option v-for="(item, key) in races" :key="key" :value="item">
                                    {{ item }}
                                </option>
                            </select>
                        </div>
                        <div class="form__group">
                            <label class="form__label" for="race">Klasse:</label>
                            <select class="dropdown" v-model="selectedClass">
                                <option value="">Velg klasse</option>
                                <option v-for="(item, key) in classes" :key="key" :value="item">
                                    {{ item }}
                                </option>
                            </select>
                        </div>
                        <div class="form__group">
                            <label class="form__label" for="race">Nivå:</label>
                            <input
                            class="form__input"
                            type="number"
                            v-model="levelInput"
                            aria-label="search term"
                            placeholder="Nivå"
                        />
                        </div>
          
                    </form>
                </template>
                <template #actions>
                    <button class="btn btn--blue modal-default-button" @click="save()">
                        Lagre
                    </button>
                    <button class="btn modal-default-button" @click="close()">Avbryt</button>
                </template>
            </modal>
        </Teleport>
    </div>
</template>
