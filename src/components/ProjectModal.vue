<template>
  <div class="project-modal" v-if="isOpen"
     :style="{ top: modalTop + 'px', left: modalLeft + 'px' }">
  <div class="modal-header" @mousedown="startDrag">
    <p>{{ folderName }}</p>
    <button class="close-button" @click="closeModal">X</button>
  </div>
  <div class="project-grid">
    <div
      class="project-item"
      v-for="project in projects"
      :key="project.name"
      @click="openProject(project.link)"
    >
      <img :src="project.icon" alt="Project icon" />
      <p>{{ project.name }}</p>
    </div>
  </div>
</div>

  </template>
  
  ```javascript
  <script setup>
  import { ref, defineProps } from 'vue'
  
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


  const isOpen = ref(false)
  
  defineProps({
    folderName: String,
    projects: Array
  })
  
  const openModal = () => {
    isOpen.value = true
  }
  
  const closeModal = () => {
    isOpen.value = false
  }

//escape key handling TODO

  
  const openProject = (link) => {
    window.open(link, '_blank')
  }
  
  defineExpose({ openModal })
  </script>
  
  <style scoped>
  .project-modal {
    position: fixed;
    top: 10%;
    left: 20%;
    width: 60%;
    height: 60%;
    background-color: #ececec;
    border: 3px solid #1c5aa6;
    border-radius: 8px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.6);
    z-index: 20;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    cursor: grab;
  }

  .project-modal:active {
  cursor: grabbing;
}


  
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: linear-gradient(to bottom, #1d65b7, #1c5aa6);
    font-family: 'Tahoma', sans-serif;
    background-color: #135aa2;
    color: white;
    padding: 0.5rem 1rem;
    font-size: 20px;
    font-weight: bold;
    border-bottom: 2px solid #0d3d7a;
  }
  

  .project-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
    gap: 1.5rem;
    padding: 1rem;
  }
  

  .project-item {
    text-align: center;
    cursor: pointer;
    font-family: 'Courier New', Courier, monospace;
    
  }


  .project-item p {
  margin: 0;
  font-size: 14px;
  color: black;
}
  

  .project-item img {
    width: 60px;
    margin-bottom: 0.5rem;
  }

  
  .project-item:hover {
    transform: scale(1.1);
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
  