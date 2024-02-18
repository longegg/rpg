<script setup lang="ts">
import type { Character } from '@/types/Character';
const props = defineProps<{
        character: Character
    }>();
</script>

<template>
    <div>
    {{ props.character.name }}
</div>
</template>