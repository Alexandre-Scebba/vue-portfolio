<template>
    <div class="project-modal" v-if="isOpen">
      <div class="modal-header">
        <p>{{ folderName }}</p>
        <button @click="closeModal">✖</button>
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
  
  <script setup>
  import { ref, defineProps } from 'vue'
  
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
    background-color: #ddd;
    border: 3px solid #555;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
    z-index: 20;
    display: flex;
    flex-direction: column;
  }
  
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #135aa2;
    color: white;
    padding: 0.5rem 1rem;
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
  }
  
  .project-item img {
    width: 60px;
    margin-bottom: 0.5rem;
  }
  
  .project-item:hover {
    transform: scale(1.1);
  }
  </style>
  