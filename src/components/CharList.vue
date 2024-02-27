<script setup lang="ts">
import type { Character } from '@/types/Character';

    const props = defineProps<{
        characters: Character[]
    }>();
</script>

<template>
    <div class="list">
        <div v-for="c in props.characters" v-bind:key="c.name">{{c.name}}</div>
    </div>
</template>

<style scoped>
.filters {
    margin-top: 30px;
}
.list {
    margin-top: 30px;
}
</style>