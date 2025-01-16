<template>
    <div class="bsod-overlay" v-if="isVisible" @click="dismissBSOD">
      <p class="bsod-text">
        A problem has been detected and your system has been shut down to prevent damage...
      </p>
    </div>
  </template>
  
  <script setup>
  import { ref } from "vue";
  
  const isVisible = ref(false);
  
  const showBSOD = () => {
    isVisible.value = true;
  };
  
  const dismissBSOD = () => {
    isVisible.value = false;
  };
  
  // Expose function to parent
  defineExpose({ showBSOD });
  </script>
  
  <style scoped>
  .bsod-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: #000080; /* BSOD blue */
    color: white;
    font-family: "Courier New", monospace;
    font-size: 16px;
    line-height: 1.5;
    z-index: 1000;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
  
  .bsod-text {
    max-width: 80%;
  }
  </style>
  