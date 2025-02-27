<script setup lang="ts">
import { ref, markRaw } from 'vue'
import Container from '@/components/Container.vue'
import HeroShardCalculator from '@/components/HeroShardCalculator.vue'
import UniqueWeaponShardCalculator from '@/components/UniqueWeaponShardCalculator.vue'
import CastleLevelRequire from '@/components/CastleLevelRequire.vue'
import Awakening from '@/components/Awakening.vue'

const tabItems = ref([
  {
    id: 'heroes-shard-calculator',
    title: 'Heroes Shard Calculator',
    content: markRaw(HeroShardCalculator),
    isActive: true,
  },
  {
    id: 'unique-weapon-shard-calculator',
    title: 'Unique Weapon Shard Calculator',
    content: markRaw(UniqueWeaponShardCalculator),
    isActive: false,
  },
  {
    id: 'castle-level-required',
    title: 'Castle Level Requirement',
    content: markRaw(CastleLevelRequire),
    isActive: false,
  },
  {
    id: 'awakening-calculator',
    title: 'Awakening Calculator',
    content: markRaw(Awakening),
    isActive: false,
  }
]);
const handleSwitchTab = (e: MouseEvent) => {
  const _self = e.currentTarget as HTMLAnchorElement;
  const targetID = _self.dataset.target ?? '';
  const target = document.getElementById(targetID);
  tabItems.value.forEach((item) => {
    item.isActive = item.id === targetID;
  });  
}
</script>
<template>
  <nav>
    <Container :fluid="false">
      <ul class="flex gap-2 flex-wrap justify-center">
        <li v-for="item of tabItems" :key="item.id">
          <a class="block px-4 py-2 bg-sky-500 text-white rounded transition-colors duration-300" :class="{ 'bg-sky-700': item.isActive }" href="javascript:void(0);" :data-target="item.id" @click="handleSwitchTab">{{ item.title }}</a>
        </li>
      </ul>
    </Container>
  </nav>
  <div class="h-auto overflow-y-auto">
    <div v-for="item of tabItems" :key="item.id" :id="item.id" v-show="item.isActive" class="h-auto overflow-y-auto">
      <component :is="{...item.content}" />
    </div>
  </div>
</template>