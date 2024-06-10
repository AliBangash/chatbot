<template>
  <div class="modal-overlay" @click.self="closeModal">
    <div class="modal-content">
      <p>All messages in the chat will be cleared. Would you like to clear this conversation?</p>
      <div class="modal-actions">
        <button @click="clearChat">Clear</button>
        <button @click="closeModal">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue';

const emits = defineEmits(['clear-chat', 'close-modal']);

const clearChat = () => {
  emits('clear-chat');
};

const closeModal = () => {
  emits('close-modal');
};
</script>

<style scoped>
.modal-overlay {
  position: absolute; 
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  color: #000;
  width: 90%;
}

.modal-actions {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 20px;
}

button {
  padding: 10px 20px;
  border: none;
  cursor: pointer;
}

button:first-of-type {
  background-color: red;
  color: white;
}

button:last-of-type {
  background-color: gray;
  color: white;
}
</style>
