<script setup lang="ts">
import Container from '@/components/Container.vue'
import awakeningData from '@/api/awakening.json'

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
</script>
<template>
  <section class="py-8 h-full overflow-hidden">
    <Container :class="`overflow-y-auto`">
      <h2 class="text-3xl font-bold text-center">Legendary Hero Awakening</h2>
      <div class="py-4 flex justify-center w-full overflow-y-auto h-[78vh]">
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
  </section>
</template>