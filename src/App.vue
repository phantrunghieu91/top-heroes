<script setup lang="ts">
import bgImg from '@/assets/top-hero-bg.webp';
import Header from './components/Header.vue';
import Tabs from './components/Tabs.vue';
</script>

<template>
  <div :style="{ backgroundImage: `url(${bgImg})` }" id="content" class="h-svh overflow-hidden bg-cover bg-center">
    <div class="relative z-[2] h-full overflow-hidden">
      <Header />
      <Tabs />
    </div>
  </div>
</template>

<style scoped>
#content {
  position: relative;

  &:before {
    content: '';
    position: absolute;
    z-index: 1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgb(from #e1e1e1 r g b / 60%);
  }
}
</style>