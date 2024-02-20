<script setup lang="ts">
import type { Character } from '@/types/Character';
import { useDateFormat } from '@vueuse/core'

const formatDate = (date: any) => useDateFormat(date, 'DD.MM.YYYY', { locales: 'nb-NO' })

const props = defineProps<{
    character: Character
}>();
</script>

<template>
<tr>
    <td>{{ props.character.name }}</td>
    <td>{{ props.character.race }}</td>
    <td>{{ props.character.className }}</td>
    <td>{{ props.character.level }}</td>
    <td>{{ formatDate(props.character.createdAt) }}</td>
</tr>
</template>