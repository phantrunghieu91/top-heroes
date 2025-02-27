<script lang="ts" setup>
import heroStars from '@/assets/heroes-stars.jpg';
import Container from './Container.vue';
import InputGroup from './InputGroup.vue';

import { ref } from 'vue';
const tier = ref('legendary');
const shardPerStep = ref([0, 1, 1, 2, 2, 4, 6, 4, 4, 8, 8, 16, 4, 8, 16, 16]);
const stars = ref({
  start: 0,
  end: 0,
});
const totalShards = ref(0);
const fullStarStep = 5;
const calculateShards = () => {
  let start = stars.value.start;
  let end = stars.value.end;
  
  if(start === end) {
    totalShards.value = 0;
    return;
  }

  if(start > end) {
    totalShards.value = 0;
    alert('Current star should be less than wanted star');
    return;
  }
  // if end is decimal, round it to the nearest whole number
  if(end % 1 > 0) {
    end = Math.ceil(end);
    stars.value.end = end;
  }

  // start and end decimal part cant greater than 5
  if(start % 1 > 0.5 || end % 1 > 0.5) {
    totalShards.value = 0;
    alert('Star decimal part should be less than 0.5');
    // reset the input
    stars.value = { start: 0, end: 0 };
    return;
  }

  let total = 0;
  let currentStar = Math.floor(start);
  let wantedStar = Math.floor(end);
  let currentStarDecimal = Math.floor(start % 1 * 10);
  if(currentStarDecimal === 5) {
    currentStarDecimal = 0;
    stars.value.start = currentStar + 1;
    currentStar += 1;
  }    
  // console.log('currentStarDecimal: ', currentStarDecimal);
  for(let i = currentStarDecimal === 0 ? currentStar + 1 : currentStar; i <= wantedStar; i++) {
    let tierMulti = tier.value === 'legendary' ? 1 : 2;
    let shard = shardPerStep.value[i];
    let shardPerStar = shard * fullStarStep * tierMulti;
    let remainingShardForCurrentStar = 0;
    if(i === currentStar && currentStarDecimal > 0) {
      remainingShardForCurrentStar = currentStarDecimal * shard * tierMulti;
      console.log('have remain: ', remainingShardForCurrentStar);
      shardPerStar -= remainingShardForCurrentStar;
    }
    // console.log(`${i}: `, shardPerStar, remainingShardForCurrentStar);
    total += shardPerStar;
  }

  totalShards.value = total;
}
const resetInputs = () => {
  stars.value = { start: 0, end: 0 };
  totalShards.value = 0;
}
</script>
<template>
  <section class="py-8 h-full overflow-hidden">
    <Container :class="`h-[80svh] overflow-y-auto`">
      <h2 class="text-3xl font-bold">Heroes Shard Calculator</h2>
      <div class="py-8 grid grid-cols-3 gap-2">
        <InputGroup label="Select Tier" id="tier" type="select" :options="['legendary', 'mythic']" v-model="tier"/>
        <InputGroup label="Enter current star (e.g, 5.1)" id="current-star" type="number" v-model="stars.start" :number-options="{step : 0.1, min: 0, max: 15}"/>
        <InputGroup label="Enter your wanted star (e.g, 15)" id="wanted-star" type="number" v-model="stars.end" :number-options="{step : 0.1, min: 0, max: 15}"/>
        <div class="col-span-3 bg-white p-4 rounded shadow grid grid-cols-2 md:grid-cols-4 gap-4">
          <span class="capitalize">Hero Tier: {{ tier }}</span>
          <span>Current Star: {{ stars.start }}</span>
          <span>Wanted Star: {{ stars.end }}</span>
          <span>Total Shards: {{ totalShards }}</span>
        </div>
        <div class="flex justify-center gap-4 col-span-3">
          <button 
            type="button" 
            class="px-4 py-2 bg-blue-500 text-white rounded cursor-pointer"
            @click="calculateShards"
          >Calculate</button>
          <button type="reset" class="px-4 py-2 bg-gray-400 rounded cursor-pointer" @click="resetInputs">Reset</button>
        </div>
      </div>
      <div class="flex justify-center">
        <img :src="heroStars" alt="Heroes Stars" class="w-fit" />
      </div>
    </Container>
  </section>
</template>
