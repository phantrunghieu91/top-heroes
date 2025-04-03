<script setup lang="ts">
import Container from '@/components/Container.vue'
import TabsContentHeading from './TabsContentHeading.vue';
import TabsContentFormWrapper from './TabsContentFormWrapper.vue';
import FormButtonGroup from './FormButtonGroup.vue';
import gearUpgrade from '@/api/gear-upgrade.json'
import { ref } from 'vue';

type EnhanceOptions = '0' | '10' | '20' | '30' | '40' | '45' | '50' | '55' | '60' | '65';

type Enhance = {
  start: EnhanceOptions,
  end: EnhanceOptions
}

const enhance = ref<Enhance>({
  start: '0',
  end: '10'
});
const total = ref({
  rune: 0,
  gold_ingot: 0,
  legendary_promotion_stone: 0,
  mythic_promotion_stone: 0
});
const labels = {
  '0': '0',
  '10': '10',
  '20': '20',
  '30': '30',
  '40': '40',
  '45': 'First Star',
  '50': 'Second Star',
  '55': 'Third Star',
  '60': 'Fourth Star',
  '65': 'Fifth Star'
}
const colNum = 2;

const calculate = () => {
  let start: number = parseInt(enhance.value.start);
  let end: number = parseInt(enhance.value.end);
  if (start >= end) {
    alert('Wanted level should be greater than current level');
    return;
  }
  let totalRune = 0;
  let totalGoldIngot = 0;
  let totalLegendaryPromotionStone = 0;
  let totalMythicPromotionStone = 0;

  for (let i = start; i < (end <= 40 ? end : 40); i++) {
    totalRune += gearUpgrade.gear_enhance[i].rune;
  }

  if(end <= 40) {
    total.value.rune = totalRune;
    return;
  }

  if (end > 40) {
    for (let i = 41; i <= end; i++) {
      const index = i < 46 ? 0 : i < 51 ? 1 : i < 56 ? 2 : i < 61 ? 3 : 4;
      const promotionData = gearUpgrade.gear_promotion[index];
      totalRune += promotionData.rune;
      totalGoldIngot += promotionData.gold_ingot;
      if (i % 5 === 0) {
        totalLegendaryPromotionStone += promotionData.legendary_promotion_stone ?? 0;
        totalMythicPromotionStone += promotionData.mythic_promotion_stone ?? 0;
      }
    }
  }
  total.value.rune = totalRune;
  total.value.gold_ingot = totalGoldIngot;
  total.value.legendary_promotion_stone = totalLegendaryPromotionStone;
  total.value.mythic_promotion_stone = totalMythicPromotionStone;
}
const resetInputs = () => {
  enhance.value.start = '0';
  enhance.value.end = '10';
}
// console.log(gearUpgrade)
</script>
<template>
  <container>
    <TabsContentHeading title="Gear Upgrade Calculator" />
    <TabsContentFormWrapper :col-num="colNum">
      <div class="bg-white p-4 rounded shadow">
        <label for="start-level" class="block mb-2">Start Level</label>
        <select id="start-level" name="start-level" v-model="enhance.start"
          class='w-full p-2 border border-gray-300 rounded bg-gray-100'>
          <option value="0">0</option>
          <option value="10">10</option>
          <option value="20">20</option>
          <option value="30">30</option>
          <option value="40">40</option>
          <option value="45">{{ labels['45'] }}</option>
          <option value="50">{{ labels['50'] }}</option>
          <option value="55">{{ labels['55'] }}</option>
          <option value="60">{{ labels['60'] }}</option>
        </select>
      </div>
      <div class="bg-white p-4 rounded shadow">
        <label for="end-level" class="block mb-2">Wanted Level</label>
        <select id="end-level" name="end-level" v-model="enhance.end"
          class='w-full p-2 border border-gray-300 rounded bg-gray-100'>
          <option value="10">10</option>
          <option value="20">20</option>
          <option value="30">30</option>
          <option value="40">40</option>
          <option value="45">{{ labels['45'] }}</option>
          <option value="50">{{ labels['50'] }}</option>
          <option value="55">{{ labels['55'] }}</option>
          <option value="60">{{ labels['60'] }}</option>
          <option value="65">{{ labels['65'] }}</option>
        </select>
      </div>
      <div class="col-span-1 md:col-span-2 bg-white p-4 rounded shadow grid grid-cols-1 md:grid-cols-2 gap-4">
        <span>Current Level: {{ labels[enhance.start] }}</span>
        <span>Wanted Level: {{ labels[enhance.end] }}</span>
        <span class="text-rose-700 md:col-span-2 justify-self-center">Total: {{ total.rune }} runes, {{ total.gold_ingot
        }} gold ingots, {{
            total.legendary_promotion_stone }} Legendary Promotion Stone, {{ total.mythic_promotion_stone }} Mythic
          Promotion Stone</span>
      </div>
      <FormButtonGroup :calculate="calculate" :reset="resetInputs" :col-num="colNum" />
    </TabsContentFormWrapper>
  </container>
</template>