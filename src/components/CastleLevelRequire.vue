<script setup lang="ts">
import castleRequirement from '@/api/castle-requirement.json';
import Container from './Container.vue';
import TabsContentHeading from './TabsContentHeading.vue';

type required = {
  level: number;
  resourcesRequired: {
    timber_stone: number;
    dragon_essence: number;
    ruby: number;
  },
  buildingsRequired: [
    {
      building: 'Research Cottage' | 'Hospital' | 'Training Ground' | 'Guild' | 'League Barrack' | 'Brilliance';
      level: number;
    }
  ]
};
const cellClasses = 'border border-blue-900 p-2 bg-gray-100 text-center';
const headerClasses = 'border border-blue-900 px-2 py-4 bg-sky-100 text-center';

</script>

<template>
  <Container>
    <TabsContentHeading :title="'Castle Level Requirement'" />
    <div class="py-4 flex justify-center w-full">
      <table class="border-collapse border-2 border-blue-900">
        <thead>
          <tr>
            <th :class="headerClasses">Castle Level</th>
            <th :class="headerClasses">Resources Required</th>
            <th :class="headerClasses">Buildings Required</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="requirement, idx of castleRequirement" :key="idx">
            <td :class="cellClasses">{{ requirement.level }}</td>
            <td :class="cellClasses">
              <ul class="flex gap-2 md:gap-4 flex-col md:flex-row md:justify-center">
                <li>Timber & Stone: {{ requirement.resourcesRequired.timber_stone }}M</li>
                <li v-if="requirement.resourcesRequired.ruby > 0">Ruby: {{ requirement.resourcesRequired.ruby }}M</li>
                <li v-if="requirement.resourcesRequired.dragon_essence > 0">Dragon Essence: {{
                  requirement.resourcesRequired.dragon_essence }}</li>
              </ul>
            </td>
            <td :class="cellClasses">
              <ul class="flex flex-col gap-2 items-center">
                <li v-for="building of requirement.buildingsRequired" :key="building.building">
                  {{ building.building }}: {{ building.level }}
                </li>
              </ul>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </Container>
</template>