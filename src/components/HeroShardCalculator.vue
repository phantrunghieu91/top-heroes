<script lang="ts" setup>
import heroStars from '@/assets/heroes-stars.jpg';
import Container from './Container.vue';
import InputGroup from './InputGroup.vue';
import TabsContentHeading from './TabsContentHeading.vue';
import TabsContentFormWrapper from './TabsContentFormWrapper.vue';

import { ref } from 'vue';
const tier = ref('legendary');
const shardPerStep = ref([1, 1, 2, 2, 4, 6, 4, 4, 8, 8, 16, 4, 8, 16, 16]);
const stars = ref({
  start: 0,
  process: 0,
  end: 0,
});
const totalShards = ref(0);
const fullStarStep = 5;
const calculateShards = () => {
  let start = stars.value.start;
  let end = stars.value.end;
  let process = stars.value.process;

  if (start === end) {
    totalShards.value = 0;
    return;
  }

  if (start > end) {
    totalShards.value = 0;
    alert('Current star should be less than wanted star');
    return;
  }

  let total = 0;
  
  for (let i = start; i < end; i++) {
    let tierMulti = tier.value === 'legendary' ? 1 : 2;
    let shard = shardPerStep.value[i];
    let shardPerStar = shard * fullStarStep * tierMulti;
    let remainingShardForCurrentStar = 0;
    if (i === start && process > 0) {
      remainingShardForCurrentStar = process * shard * tierMulti;
      shardPerStar -= remainingShardForCurrentStar;
    }
    total += shardPerStar;
  }

  totalShards.value = total;
}
const resetInputs = () => {
  stars.value = { start: 0, process: 0, end: 0 };
  totalShards.value = 0;
}
const colNum = 4;
</script>
<template>
  <Container>
    <TabsContentHeading :title="'Heroes Shard Calculator'"/>
    <TabsContentFormWrapper :col-num="colNum">
      <InputGroup label="Select Tier" id="tier" type="select" :options="['legendary', 'mythic']" v-model="tier" />
      <InputGroup label="Enter current star" id="current-star" type="number" v-model="stars.start"
        :number-options="{ step: 1, min: 0, max: 15 }" />
      <InputGroup label="Current star process (eg. 1 / 5 of star)" id="current-star-process" type="select" :options="['0', '1', '2', '3', '4']" v-model="stars.process"/>
      <InputGroup label="Enter your wanted star (e.g, 15)" id="wanted-star" type="number" v-model="stars.end"
        :number-options="{ step: 1, min: 0, max: 15 }" />
      <div class="col-span-2 md:col-span-4 bg-white p-4 rounded shadow grid grid-cols-2 md:grid-cols-4 gap-4">
        <div class="capitalize">Hero Tier: <span :class="tier === 'legendary' ? 'text-amber-500' : 'text-red-600'">{{ tier }}</span></div>
        <span>Current Star: {{ stars.start }}</span>
        <span>Wanted Star: {{ stars.end }}</span>
        <span class="text-rose-700">Total Shards: {{ totalShards }}</span>
      </div>
      <div class="flex justify-center gap-4 col-span-2 md:col-span-4">
        <button type="button" class="px-4 py-2 bg-blue-500 text-white rounded cursor-pointer"
          @click="calculateShards">Calculate</button>
        <button type="reset" class="px-4 py-2 bg-gray-400 rounded cursor-pointer" @click="resetInputs">Reset</button>
      </div>
    </TabsContentFormWrapper>
    <div class="flex justify-center">
      <img :src="heroStars" alt="Heroes Stars" class="w-fit" />
    </div>
  </Container>
</template>
