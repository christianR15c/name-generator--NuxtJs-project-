<template>
  <div class="card" @click="removeName">
    <p>{{ name }}</p>
  </div>
</template>

<script setup lang="ts">
interface NameProps {
  name: string;
  index: number;
}
const props = defineProps<NameProps>();

const emit = defineEmits(['remove']);

const removeName = () => {
  emit('remove', props.index);
};
</script>

<style>
.card {
  width: 28%;
  border-radius: 1rem;
  margin-right: 0.5rem;
  margin-bottom: 1rem;
  background-color: rgb(27, 60, 138);
  color: white;
  cursor: pointer;
}
</style>
