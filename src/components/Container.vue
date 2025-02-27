<script lang="ts" setup>
import { computed } from 'vue';
const props = defineProps<{
  fluid?: boolean;
  classes?: string;
}>();
const containerClasses = props.fluid ? `${props.classes ?? ''} max-w-full` : `${props.classes ?? ''} max-w-7xl`;
</script>
<template>
  <div class="mx-auto px-2.5 lg:px-0" :class="containerClasses">
    <slot></slot>
  </div>
</template>