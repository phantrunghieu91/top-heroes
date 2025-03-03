<script setup lang="ts">
import { computed } from 'vue'
type PropType = {
  colNum?: 2 | 3 | 4;
}
const props = defineProps<PropType>();

const classes = computed(() => {
  switch (props.colNum) {
    case 2:
      return 'grid-cols-1 md:grid-cols-2';
    case 3:
      return 'grid-cols-1 md:grid-cols-3';      
    case 4:
    default:
      return 'grid-cols-2 md:grid-cols-4';
  }
});
</script>
<template>
  <div class="py-4 grid gap-2" :class="classes">
    <slot></slot>
  </div>
</template>