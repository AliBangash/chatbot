<template>
  <div>
    <ChatIcon @toggle-chat="toggleChatBoard" />
    <div :style="{ display: isChatBoardVisible ? 'block' : 'none' }" id="chatBoard">
      <!-- <ChatHeader @toggle-chat="toggleChatBoard" @clear-chat="resetChatBoard" /> -->
      <ChatContent />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import ChatIcon from '~/components/ChatIcon.vue';
import ChatHeader from '~/components/ChatHeader.vue';
import ChatContent from '~/components/ChatContent.vue';

const isChatBoardVisible = ref(false);

const toggleChatBoard = () => {
  isChatBoardVisible.value = !isChatBoardVisible.value;
  if (!isChatBoardVisible.value) {
    resetChatBoard();
  }
};

const resetChatBoard = () => {
  // Reset chat logic if necessary
  console.log('Chat cleared');
};
</script>


<style scoped>
#chatBoard {
  position: fixed !important;
  bottom: 70px;
  right: 20px;
  width: 370px;
  height: 550px;
  background: white;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
  overflow-y: auto;
  position: relative;
}

#chatBoard::-webkit-scrollbar {
  width: 5px;
}

#chatBoard::-webkit-scrollbar-track {
  background: #f1f1f1;
}

#chatBoard::-webkit-scrollbar-thumb {
  background: #888;
}

#chatBoard::-webkit-scrollbar-thumb:hover {
  background: #555;
}
#chatBoard::-webkit-scrollbar {
  width: 5px;
}

#chatBoard::-webkit-scrollbar-track {
  background: #f1f1f1;
}

#chatBoard::-webkit-scrollbar-thumb {
  background: #888;
}

#chatBoard::-webkit-scrollbar-thumb:hover {
  background: #555;
}

.chat-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #00408a;
  color: white;
  border-radius: 8px 8px 0 0;
  position: sticky;
  top: 0;
  z-index: 10;
  height: 60px; /* Ensure this matches padding-top in #chatBoard */
}
</style>
