<script setup lang="ts">
type Props = {
  label: string;
  id: string;
  type: 'number' | 'select';
  options?: string[];
  numberOptions?: {
    min: number;
    max: number;
    step: number;
  };
};
const props = defineProps<Props>()
const model = defineModel()
const classes = 'w-full p-2 border border-gray-300 rounded bg-gray-100 leading-0 self-end'
</script>
<template>
  <div class="bg-white p-4 rounded shadow flex flex-col h-full justify-between">
    <label :for="id" class="block mb-2">{{ label }}</label>
    <input v-if="type == 'number'" v-model="model" :id="id" type="number" :step="numberOptions?.step" :min="numberOptions?.min" :max="numberOptions?.max" required
      :class="classes" />
    <select v-model="model" :id="id" :class="classes" v-if="type == 'select'" class="capitalize">
      <option v-for="option in options" :key="option" :value="option">{{ option }}</option>
    </select>
  </div>
</template>