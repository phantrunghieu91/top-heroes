<script setup lang="ts">
import Container from '@/components/Container.vue'
import InputGroup from './InputGroup.vue';
import awakeningData from '@/api/awakening.json'

import { ref } from 'vue';
import TabsContentHeading from './TabsContentHeading.vue';
import TabsContentFormWrapper from './TabsContentFormWrapper.vue';

const calcTotal = ref({
  shard: 0,
  soulStone: 0,
});
const current = ref({
  tier: '0',
  level: '0',
});
const wanted = ref({
  tier: '1',
  level: '0',
});

/*
  - cellClasses: A string containing CSS classes for styling table cells. 
  - headerClasses: A string containing CSS classes for styling table headers.
*/
const cellClasses = 'border border-blue-900 px-2 py-1 bg-gray-100 text-center';
const headerClasses = 'border border-blue-900 px-2 py-4 bg-sky-100 text-center';

const total = {
  shard: 0,
  soulStone: 0,
}

awakeningData.forEach((awaken) => {
  total.shard += awaken.awaken.shard;
  awaken.process.forEach((process) => {
    total.shard += process.shard;
  });
  total.soulStone += awaken.awaken.soul_stone;
})

const calculateShards = () => {
  if( wanted.value.tier < current.value.tier ) {
    alert('Wanted tier should be greater than current tier');
    return;
  }  
  if( wanted.value.tier === current.value.tier && wanted.value.level <= current.value.level ) {
    alert('Wanted level should be greater than current level');
    return;
  }

  let totalShard = 0;
  let totalSoulStone = 0;
  let currentTier = parseInt(current.value.tier);
  let currentLevel = parseInt(current.value.level);
  let wantedTier = parseInt(wanted.value.tier);
  let wantedLevel = wantedTier == 4 || wanted.value.level == '0' ? 5 : parseInt(wanted.value.level);
  
  for( let i = currentTier; i < wantedTier; i++ ) {
    const currentAwaken = awakeningData.find(awaken => awaken.tier === i);
    if( !currentAwaken ) {
      alert('Awaken not found');
      return;
    }
    totalSoulStone += currentAwaken.awaken.soul_stone;
    if( i > 0 ) {
      // console.log('i: ', i,'currentLevel: ', currentLevel, 'wantedLevel: ', wantedLevel);
      for( let j = currentLevel + 1; j <= wantedLevel; j++ ) {
        const currentProcess = currentAwaken.process.find(process => process.level === j);
        if( !currentProcess ) {
          alert('Process not found');
          return;
        }
        totalShard += currentProcess.shard;
      }
    }
    totalShard += currentAwaken.awaken.shard;
    currentLevel = 0;
  }
  calcTotal.value = { shard: totalShard, soulStone: totalSoulStone }
}
const resetInputs = () => {
  current.value = { tier: '0', level: '0' };
  wanted.value = { tier: '1', level: '0' };
  calcTotal.value = { shard: 0, soulStone: 0 };
}
</script>
<template>
  <Container>
    <TabsContentHeading :title="'Legendary Hero Awakening'" />
    <TabsContentFormWrapper>
      <InputGroup label="Current Tier" type="select" :options="['0', '1', '2', '3']" id="current-tier"
        v-model="current.tier" />
      <InputGroup label="Current Level" type="select" :options="current.tier === '0' ? ['0'] : ['0', '1', '2', '3', '4', '5']" id="current-level"
        v-model="current.level" />
      <InputGroup label="Wanted Tier" type="select" :options="['1', '2', '3', '4']" id="wanted-tier"
        v-model="wanted.tier" />
      <InputGroup label="Wanted Level" type="select" :options="wanted.tier === '4' ? ['0'] : ['0', '1', '2', '3', '4', '5']" id="wanted-level"
        v-model="wanted.level" />
      <div class="col-span-2 md:col-span-4 bg-white p-4 rounded shadow grid grid-cols-2 md:grid-cols-3 gap-4">
        <span>Current Tier: {{ current.tier }} - Current Level: {{ current.level }}</span>
        <span>Wanted Tier: {{ wanted.tier }} - Wanted Level: {{ wanted.level }}</span>
        <span class="text-rose-700">Total: {{ calcTotal.shard }} shards, {{ calcTotal.soulStone }} soul stone.</span>
      </div>
      <div class="flex justify-center gap-4 col-span-2 md:col-span-4">
        <button type="button" class="px-4 py-2 bg-blue-500 text-white rounded cursor-pointer"
          @click="calculateShards">Calculate</button>
        <button type="reset" class="px-4 py-2 bg-gray-400 rounded cursor-pointer" @click="resetInputs">Reset</button>
      </div>
    </TabsContentFormWrapper>
    <div class="flex justify-center w-full">
      <table class="border-collapse border-2 border-blue-900">
        <thead>
          <tr>
            <th :class="headerClasses">Tier</th>
            <th :class="headerClasses">Level</th>
            <th :class="headerClasses">Shard</th>
            <th :class="headerClasses">Soul Stone</th>
            <th :class="headerClasses">Tier upto</th>
          </tr>
        </thead>
        <tbody>
          <template v-for="awaken, idx of awakeningData" :key="idx">
            <template v-if="idx === 0">
              <tr>
                <td :class="cellClasses">{{ awaken.tier }}</td>
                <td :class="cellClasses">0</td>
                <td :class="cellClasses">{{ awaken.awaken.shard }}</td>
                <td :class="cellClasses">{{ awaken.awaken.soul_stone }}</td>
                <td :class="cellClasses" class="font-bold">{{ awaken.tier + 1 }}</td>
              </tr>
            </template>
            <template v-else>
              <tr v-for="process, processIdx of awaken.process" :key="`process-${processIdx}`">
                <td :class="cellClasses" v-if="processIdx === 0" rowspan="6">{{ awaken.tier }}</td>
                <td :class="cellClasses">{{ process.level }}</td>
                <td :class="cellClasses">{{ process.shard }}</td>
                <td :class="cellClasses">-</td>
                <td :class="cellClasses">-</td>
              </tr>
              <tr>
                <td :class="cellClasses">-</td>
                <td :class="cellClasses">{{ awaken.awaken.shard }}</td>
                <td :class="cellClasses">{{ awaken.awaken.soul_stone }}</td>
                <td :class="cellClasses" class="font-bold">{{ awaken.tier + 1 }}</td>
              </tr>
            </template>
          </template>
        </tbody>
        <tfoot>
          <tr>
            <td class="border border-blue-900 px-2 py-4 bg-sky-300 text-center font-bold" colspan="2">Total</td>
            <td class="border border-blue-900 px-2 py-4 bg-sky-300 text-center font-bold">{{ total.shard }}</td>
            <td class="border border-blue-900 px-2 py-4 bg-sky-300 text-center font-bold">{{ total.soulStone }}</td>
            <td class="border border-blue-900 px-2 py-4 bg-sky-300 text-center font-bold">-</td>
          </tr>
        </tfoot>
      </table>
    </div>
  </Container>
</template>