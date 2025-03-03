<script setup lang="ts">
import TabsContentHeading from './TabsContentHeading.vue';
import TabsContentFormWrapper from './TabsContentFormWrapper.vue';
import Container from './Container.vue';
import InputGroup from './InputGroup.vue';
import FormButtonGroup from './FormButtonGroup.vue';
import skillBooksData from '@/api/skill-books.json';
import { ref } from 'vue';

const currentLevel = ref(1);
const wantedLevel = ref(2);
const totalBooks = ref(0);

const calculateBooks = () => {
  if (wantedLevel.value < currentLevel.value) {
    alert('Wanted level should be greater than current level');
    return;
  }

  let total = 0;
  for (let i = currentLevel.value; i <= wantedLevel.value; i++) {
    const currentLevelCost = skillBooksData.find((book) => book.level === i);
    total += currentLevelCost?.cost ?? 0;
  }
  totalBooks.value = total;
}

const resetInputs = () => {
  currentLevel.value = 1;
  wantedLevel.value = 2;
  totalBooks.value = 0;
}

const colNum = 2;
const cellClasses = 'border border-blue-900 px-2 py-1 bg-gray-100 text-center min-w-14';
const headerClasses = 'border border-blue-900 px-2 py-4 bg-sky-100 text-center min-w-14 whitespace-nowrap';

</script>
<template>
  <Container>
    <TabsContentHeading title="Skill Books Calculator" />
    <TabsContentFormWrapper :col-num="colNum">
      <InputGroup :type="'number'" :id="'current-level'" :label="'Current level'" v-model="currentLevel" :number-options="{ min: 1, max: 14, step: 1}"/>
      <InputGroup :type="'number'" :id="'wanted-level'" :label="'Wanted level'" v-model="wantedLevel" :number-options="{ min: 2, max: 15, step: 1}"/>
      <div class="col-span-1 md:col-span-2 bg-white p-4 rounded shadow grid grid-cols-1 md:grid-cols-3 gap-4">
        <span>Current Level: {{ currentLevel }}</span>
        <span>Wanted Level: {{ wantedLevel }}</span>
        <span class="text-rose-700">Total: {{ totalBooks }} books.</span>
      </div>
      <FormButtonGroup :calculate="calculateBooks" :reset="resetInputs" :col-num="colNum"/>
    </TabsContentFormWrapper>
    <div class="lg:flex lg:justify-center w-full overflow-x-auto">
      <table class="border-collapse border-2 border-blue-900">
        <tbody>
          <tr>
            <th :class="headerClasses">Skill Level</th>
            <th :class="headerClasses" v-for="book, idx of skillBooksData" :key="idx">{{ book.level }}</th>
          </tr>
          <tr>
            <td :class="cellClasses">Cost</td>
            <td :class="cellClasses" v-for="book, idx of skillBooksData" :key="idx">{{ book.cost }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </Container>
</template>