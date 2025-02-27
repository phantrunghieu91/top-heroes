<script setup lang="ts">
import Container from './Container.vue';
import InputGroup from './InputGroup.vue';
import { ref } from 'vue';

const shardPerLevel = [5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 80, 90, 100, 110, 130, 150];

const level = ref({
  start: 0,
  end: 0,
});

const totalShards = ref(0);

const resetInputs = () => {
  level.value = { start: 0, end: 0 };
  totalShards.value = 0;
}

const calculateShards = () => {
  let start = level.value.start;
  let end = level.value.end;

  if (start === end) {
    totalShards.value = 0;
    return;
  }

  if (start > end) {
    totalShards.value = 0;
    alert('Current level should be less than wanted level');
    return;
  }

  let total = 0;
  for (let i = start; i < end; i++) {
    total += shardPerLevel[i];
  }

  totalShards.value = total;
}

const cellClasses = 'border border-gray-400 p-2 bg-gray-100 text-center';

</script>
<template>
  <Container>
    <h2 class="text-2xl md:text-3xl font-bold text-center">Unique Weapon Shard Calculator</h2>
    <div class="py-4 grid grid-cols-2 gap-2">
      <InputGroup v-model="level.start" label="Current Level" id="current-level" type="number"
        :number-options="{ min: 0, max: 20, step: 1 }" />
      <InputGroup v-model="level.end" label="Wanted Level" id="wanted-level" type="number"
        :number-options="{ min: 0, max: 20, step: 1 }" />
      <div class="col-span-2 bg-white p-4 rounded shadow grid grid-cols-3 gap-4">
        <span>Current Star: {{ level.start }}</span>
        <span>Wanted Star: {{ level.end }}</span>
        <span>Total Shards: {{ totalShards }}</span>
      </div>
      <div class="flex justify-center gap-4 col-span-2">
        <button type="button" class="px-4 py-2 bg-blue-500 text-white rounded cursor-pointer"
          @click="calculateShards">Calculate</button>
        <button type="reset" class="px-4 py-2 bg-gray-400 rounded cursor-pointer" @click="resetInputs">Reset</button>
      </div>
    </div>
    <div class="w-full overflow-x-auto">
      <table class="mx-auto mt-8 border-collapse border-2">
        <tbody>
          <tr>
            <th :class="cellClasses">Level</th>
            <td :class="cellClasses" v-for="_, idx of shardPerLevel" :key="idx">{{ idx + 1 }}</td>
          </tr>
          <tr>
            <th :class="cellClasses">Shards</th>
            <td :class="cellClasses" v-for="shard, idx of shardPerLevel" :key="idx">{{ shard }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </Container>
</template>