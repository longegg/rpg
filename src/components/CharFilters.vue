<script setup lang="ts">
import type { Character } from '@/types/Character';

    const props = defineProps<{
        filterPosts: VoidFunction,
        search: VoidFunction,
        filteredPosts: Character[]
    }>();

    const term: string = "";
</script>

<template>
      <label for="search">
    Search
    <input
      id="search"
      v-model="term"
      @keyup="props.search(term)"
    />
  </label>
</template>