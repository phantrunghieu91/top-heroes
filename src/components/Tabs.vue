<script setup lang="ts">
import { ref, markRaw, computed } from 'vue'
import HeroShardCalculator from '@/components/HeroShardCalculator.vue'
import UniqueWeaponShardCalculator from '@/components/UniqueWeaponShardCalculator.vue'
import CastleLevelRequire from '@/components/CastleLevelRequire.vue'
import AwakeningCalculator from '@/components/AwakeningCalculator.vue'
import Container from './Container.vue'
import SkillBooksCalculator from './SkillBooksCalculator.vue'

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
    content: markRaw(AwakeningCalculator),
    isActive: false,
  },
  {
    id: 'skill-books-calculator',
    title: 'Skill books Calculator',
    content: markRaw(SkillBooksCalculator),
    isActive: false,
  },
]);
const dropdownState = ref(false);
const activeTitle = computed(() => tabItems.value.find(item => item.isActive)?.title);
const handleSwitchTab = (e: MouseEvent) => {
  const _self = e.currentTarget as HTMLAnchorElement;
  const targetID = _self.dataset.target ?? '';
  tabItems.value.forEach((item) => {
    item.isActive = item.id === targetID;
  });  
  dropdownState.value = false;
}
const handleDropdownState = () => {
  dropdownState.value = !dropdownState.value;
}
</script>
<template>
  <nav class="mb-4 max-md:flex max-md:justify-center">
    <div class="relative block md:hidden">
      <span class="inline-block text-sm md:text-base px-4 py-2 bg-sky-500 text-white rounded transition-colors duration-300 cursor-pointer min-w-60" @click="handleDropdownState">{{ activeTitle }}</span>
      <ul class="flex flex-col absolute top-10 left-0 right-0 bg-white transition-opacity duration-300" :class="{ 'opacity-0 z-[-1]' : !dropdownState}">
        <li v-for="item of tabItems" :key="item.id">
          <a class="block px-4 py-1 hover:bg-sky-100" :class="{ 'bg-sky-700 text-white': item.isActive }" href="javascript:void(0);" :data-target="item.id" @click="handleSwitchTab">{{ item.title }}</a>
        </li>
      </ul>
    </div>
    <Container :fluid="false" :class="'max-md:hidden'">
      <ul class="flex gap-2 flex-wrap justify-center">
        <li v-for="item of tabItems" :key="item.id">
          <a class="block px-4 py-2 bg-sky-500 text-white rounded transition-colors duration-300" :class="{ 'bg-sky-700': item.isActive }" href="javascript:void(0);" :data-target="item.id" @click="handleSwitchTab">{{ item.title }}</a>
        </li>
      </ul>
    </Container>
  </nav>
  <div class="h-[80svh] overflow-y-auto">
    <div v-for="item of tabItems" :key="item.id" :id="item.id" v-show="item.isActive" class="h-auto overflow-y-auto">
      <component :is="{...item.content}" />
    </div>
  </div>
</template>