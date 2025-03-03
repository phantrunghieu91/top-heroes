<script lang="ts" setup>
const props = defineProps<{
  title: string
}>()
</script>

<template>
  <h2 class="text-2xl md:text-3xl font-bold text-center">{{ title }}</h2>
</template>