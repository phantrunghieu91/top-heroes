<script setup lang="ts">
import Container from './Container.vue';
</script>
<template>
  <header>
    <Container :class="`py-4`" :fluid="false">
      <h1 class="text-3xl md:text-6xl text-center font-bold text-grey-900">Top Heroes Charts</h1>
    </Container>
  </header>
</template>