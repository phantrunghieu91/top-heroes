<script setup lang="ts">
export type StarIconProps = {
  strokeWidth: number,
  strokeColor: string,
  starColor: string,
};
const props = defineProps < StarIconProps > ();
</script>
<template>
  <svg width="100" height="100" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
    <!-- Star shape -->
    <polygon points="50,10 61,38 90,38 66,58 75,85 50,70 25,85 34,58 10,38 39,38" :fill="starColor" :stroke="strokeColor"
      :stroke-width="strokeWidth" />

    <!-- Connecting lines -->
    <line x1="50" y1="10" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="61" y1="38" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="90" y1="38" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="66" y1="58" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="75" y1="85" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="50" y1="70" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="25" y1="85" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="34" y1="58" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="10" y1="38" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
    <line x1="39" y1="38" x2="50" y2="50" :stroke="strokeColor" :stroke-width="strokeWidth" />
  </svg>
</template>