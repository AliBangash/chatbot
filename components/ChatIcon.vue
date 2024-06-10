<template>
  <div id="chatIcon" @click="$emit('toggle-chat')">
    <svg viewBox="0 0 24 24" fill="#FFFFFF" width="50px" height="50px">
      <circle cx="12" cy="12" r="12" fill="#60ABFE" />
      <path
        d="M16,9H8a2,2 0 0,0 -2,2v5a2,2 0 0,0 2,2h5l3,3v-3h1a2,2 0 0,0 2,-2v-5a2,2 0 0,0 -2,-2Z"
        fill="#ffffff"
      />
    </svg>
  </div>
</template>

<script setup>
</script>

<style scoped>
#chatIcon {
  position: fixed;
  bottom: 20px;
  right: 20px;
  color: #60abfe;
  cursor: pointer;
}
</style>
