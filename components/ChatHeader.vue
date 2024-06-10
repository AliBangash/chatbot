<template>
  <div class="chat-header">
    <h1>FocusMed Assistant Bot</h1>
    <button id="closeButton" @click="handleClose">&#10006;</button>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue';

const emits = defineEmits(['close-chat']);

const handleClose = () => {
  emits('close-chat');
};
</script>

<style scoped>
.chat-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #00408abf;
  color: white;
  border-radius: 8px 8px 0 0;
  position: sticky;
  top: 0;
  z-index: 10;
}

.chat-header h1 {
  margin: 0;
  font-size: 20px;
  padding: 20px;
}

#closeButton {
  background: none;
  border: none;
  color: white;
  font-size: 20px;
  padding: 0 17px;
  cursor: pointer;
}
</style>
