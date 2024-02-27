<script setup lang="ts">
import { reactive, toRefs } from 'vue';
import modal from '../modals/ModalBox.vue';
import type { Character } from '../../types/Character';

const props = defineProps<{
    show: Boolean;
    characters: Character[];
    races: any;
    classes: any;
}>();

const { characters, races, classes } = toRefs(props);

const emit = defineEmits<{
    (e: 'close'): void;
    (e: 'save', id: number): void;
}>();

type FormData = {
    name: string;
    race: string;
    class: string;
    level: number;
};

const formData = reactive({
    name: "",
    race: races.value[0],
    class: classes.value[0],
    level: 1
} as FormData);

const close = () => emit('close');

function save() {
    characters.value.push({
        name: formData.name,
        race: formData.race,
        className: formData.class,
        level: formData.level,
        createdAt: new Date().toISOString()
    } as Character);

    emit('save', 1);
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
                                v-model="formData.name"
                                aria-label="search term"
                                placeholder=""
                            />
                        </div>
                        <div class="form__group">
                            <label class="form__label" for="race">Rase:</label>
                            <select id="race" class="dropdown" v-model="formData.race">
                                <option v-for="(item, key) in races" :key="key" :value="item">
                                    {{ item }}
                                </option>
                            </select>
                        </div>
                        <div class="form__group">
                            <label class="form__label" for="race">Klasse:</label>
                            <select class="dropdown" v-model="formData.class">
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
                            min="1"
                            max="10"
                            pattern="\d*"
                            v-model="formData.level"
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
