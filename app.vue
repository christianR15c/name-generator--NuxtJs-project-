<template>
  <div class="container">
    <h1>Baby name Generator</h1>
    <p>Choose your options and click the "Find name" button bellow</p>
    <div class="options-container">
      <Option
        v-for="(option, i) in optionsArray"
        :key="i"
        :options="options"
        :option="option"
      />
      <button class="primary" @click="computeSelectedName">Find Names</button>
    </div>
    <div class="cards-container">
      <ResultNameCard
        v-for="(name, index) in selectedNames"
        :key="index"
        :name="name"
        @remove="() => removeName(index)"
        :index="index"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { Gender, Popularity, Length, names } from './data';
import ResultNameCard from './components/cards/ResultNameCard.vue';

interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length;
}

const options: OptionsState = reactive({
  gender: Gender.GIRL,
  popularity: Popularity.TRENDY,
  length: Length.LONG,
});

const computeSelectedName = () => {
  const filterNames = names
    .filter((name) => name.gender === options.gender)
    .filter((name) => name.popularity === options.popularity)
    .filter((name) => {
      if (options.length === Length.ALL) return true;
      else return name.length === options.length;
    });
  selectedNames.value = filterNames.map((name) => name.name);
};

const selectedNames = ref<string[]>([]);

const removeName = (index: number) => {
  const filteredNames = [...selectedNames.value];
  filteredNames.splice(index, 1);
  selectedNames.value = filteredNames;
};

const optionsArray = [
  {
    title: '1) Choose a gender',
    category: 'gender',
    buttons: [Gender.GIRL, Gender.UNISEX, Gender.BOY],
  },
  {
    title: "2) Choose the name's popularity",
    category: 'popularity',
    buttons: [Popularity.TRENDY, Popularity.UNIQUE, Gender.BOY],
  },
  {
    title: "3) Choose the name's length",
    category: 'length',
    buttons: [Length.SHORT, Length.ALL, Length.LONG],
  },
];
</script>

<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}
h1 {
  font-size: 3rem;
}
.options-container {
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}

.primary {
  background-color: rgb(249, 87, 89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  margin-top: 1rem;
  cursor: pointer;
}
.cards-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
