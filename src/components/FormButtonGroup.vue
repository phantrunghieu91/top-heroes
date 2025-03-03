<script lang="ts" setup>
import { computed } from 'vue'
type Props = {
  colNum?: 2 | 3 | 4;
  calculate: () => void;
  reset: () => void;
}
const props = defineProps<Props>()

const classes = computed(() => {
  switch (props.colNum) {
    case 2:
      return 'col-span-1 md:col-span-2';
    case 3:
      return 'col-span-1 md:col-span-3';      
    case 4:
    default:
      return 'col-span-2 md:col-span-4';
  }
})
</script>
<template>
  <div class="flex justify-center gap-4" :class="classes">
    <button type="button" class="px-4 py-2 bg-blue-500 text-white rounded cursor-pointer"
      @click="calculate">Calculate</button>
    <button type="reset" class="px-4 py-2 bg-gray-400 rounded cursor-pointer" @click="reset">Reset</button>
  </div>
</template>