<template>
    <div class="modal" v-if="isOpen"
      :style="{ top: modalTop + 'px', left: modalLeft + 'px' }">
      <div class="modal-header" @mousedown="startDrag">
        <p>About Me</p>
        <button class="close-button" @click="closeModal">X</button>
      </div>
      <div class="modal-body">
        <p>
          Hello! I'm Alexandre,
          A full-stack developer with hands-on experience building interactive, 
          user-friendly web and desktop apps.
          I love coding for it's problem-solving mindset and design aspects.
          There is a rewarding challenge in crafting unique and engaging designs, using many technologies and languages 
          like React, PHP, Java, C#, & SQL-based databases.
          I'm currently getting familiar with the MERN stack,
          React Native, and TypeScript—and this portfolio was created with Vue.js.
        </p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const isOpen = ref(false)
  const modalTop = ref(150)
  const modalLeft = ref(400)
  const dragging = ref(false)
  const offsetX = ref(0)
  const offsetY = ref(0)
  
  const startDrag = (event) => {
    dragging.value = true
    offsetX.value = event.clientX - modalLeft.value
    offsetY.value = event.clientY - modalTop.value
    document.addEventListener('mousemove', onDrag)
    document.addEventListener('mouseup', stopDrag)
  }
  
  const onDrag = (event) => {
    if (dragging.value) {
      modalLeft.value = event.clientX - offsetX.value
      modalTop.value = event.clientY - offsetY.value
    }
  }
  
  const stopDrag = () => {
    dragging.value = false
    document.removeEventListener('mousemove', onDrag)
    document.removeEventListener('mouseup', stopDrag)
  }
  
  const openModal = () => {
    isOpen.value = true
  }
  
  const closeModal = () => {
    isOpen.value = false
  }
  
  defineExpose({ openModal })
  </script>
  
  <style scoped>
  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: clamp(300px, 50%, 600px);
    height: clamp(250px, 40%, 500px);
    background-color: #fff;
    border: 3px solid #333;
    border-radius: 10px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
    z-index: 20;
    display: flex;
    flex-direction: column;
    font-family: 'Courier New', Courier, monospace;
    overflow: hidden;
    cursor: grab;
  }

  .modal:active {
  cursor: grabbing;
}

  
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #135aa2;
    color: white;
    padding: 0.5rem 1rem;
    font-weight: bold;
  }
  
  .modal-body {
  padding: 1.5rem;
  text-align: left;
  color: black;
  font-size: 14px;
  line-height: 1.6;
  max-height: 70%;
  overflow-y: auto;
  max-height: 65%;
}
  
.close-button {
  background: #f35454;
  border: 1px solid #888;
  color: white;
  width: 30px;
  height: 30px;
  cursor: pointer;
  border-radius: 6px;
  text-align: center;
  font-size: 20px;
  line-height: 21px;
  font-family: 'Tahoma', sans-serif;
}

.close-button:hover {
  background: #f35454;
  color: white;
  border-color: azure;
  border-width: 2px;
}
  </style>
  