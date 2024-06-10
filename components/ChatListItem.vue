<template>
  <li @click="handleClick">{{ question.text }}</li>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  question: Object,
});

const emits = defineEmits(['show-answer']);

const handleClick = () => {
  emits('show-answer', props.question.text);
};
</script>

<style scoped>
li {
  padding: 8px;
  background-color: #f7f7f7;
  margin: 5px;
  cursor: pointer;
  color: #00408a;
  border-radius: 10px;
}

li:hover {
  background-color: #e0e0e0;
}
</style>
