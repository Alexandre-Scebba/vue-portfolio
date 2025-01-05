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
      @mouseenter="showTooltip(project, $event)"
      @mouseleave="hideTooltip"
      @click="openProject(project.link)"
    >
      <img :src="project.icon" alt="Project icon" />
      <p>{{ project.name }}</p>
    </div>
  </div>

      <!-- Tooltip -->
  <div v-if="tooltip.visible" 
       class="tooltip"
       :style="{ top: tooltip.y + 'px', left: tooltip.x + 'px' }">
    <p>{{ tooltip.text }}</p>
  </div>
</div>
  </template>
  


  ```javascript
  <script setup>
  import { ref, defineProps } from 'vue'
  
const modalTop = ref(120)
const modalLeft = ref(140)
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


  const adjustModalSize = () => {
  const modal = document.querySelector('.project-modal');
  if (modal) {
    const screenWidth = window.innerWidth;
    const screenHeight = window.innerHeight;

    // Scale modal based on screen width
    if (screenWidth < 600) {
      modal.style.width = '90%';
      modal.style.height = '70%';
      modal.style.left = '5%';
      modal.style.top = '15%';
    } else {
      modal.style.width = '60%';
      modal.style.height = '60%';
      modal.style.left = '20%';
      modal.style.top = '10%';
    }
  }
};
// listen for window resize
window.addEventListener('resize', adjustModalSize);


//escape key handling TODO

  
  const openProject = (link) => {
    window.open(link, '_blank')
  }
  
  defineExpose({ openModal })


  //hover tooltip logic
  const tooltip = ref({
  visible: false,
  text: '',
  x: 0,
  y: 0
})

  
  const showTooltip = (project, event) => {

  const modalRect = event.currentTarget.closest('.project-modal').getBoundingClientRect();
  const screenRect = document.querySelector('.desktop').getBoundingClientRect();

  // Temporarily show tooltip to measure its size
  tooltip.value.text = getProjectInfo(project.name);
  tooltip.value.visible = true;

  requestAnimationFrame(() => {
    const tooltipEl = document.querySelector('.tooltip');
    const tooltipWidth = tooltipEl.offsetWidth;
    const tooltipHeight = tooltipEl.offsetHeight;

    let x = event.clientX - screenRect.left + 15;
    let y = event.clientY - screenRect.top + 15;

    // Prevent overflow right
    if (x + tooltipWidth > screenRect.width) {
      x = screenRect.width - tooltipWidth - 15;
    }
    
    // Prevent overflow left
    if (x < 0) {
      x = 15;
    }
    
    // Prevent overflow bottom
    if (y + tooltipHeight > screenRect.height) {
      y = screenRect.height - tooltipHeight - 15;
    }
    
    // Prevent overflow top
    if (y < 0) {
      y = 15;
    }

    tooltip.value.x = x;
    tooltip.value.y = y;
  });
};


const hideTooltip = () => {
  tooltip.value.visible = false
}

const getProjectInfo = (projectName) => {
  switch (projectName) {
    case 'Portfolio Website':
      return 'A portfolio showcasing my development skills and projects.';
    case 'Examina':
      return 'A C#-based exam management web app for teachers and students.';
    case 'CODEX':
      return 'Snippet Manager built in C# to organize and store code snippets.';
    case 'DragonNotes':
      return 'Voice note app using the MERN stack and Web Speech API.';
    case 'Clonify':
      return 'Spotify clone using React and Node.js for learning purposes.';
    case 'Floppy Code':
      return 'Flappy Bird clone in HTML5 and JavaScript.';
    case 'Flash Card App':
      return 'Flashcard app to help with learning and studying.';
    default:
      return 'No additional information available.';
  }
}

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

    overflow: hidden;
    max-width: 800px;
    min-width: 300px;
    max-height: 90vh;
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
    overflow-y: auto;
    flex-grow: 1;
    max-height: 100%;
  }

  /* modal scaling for smaller screens */
@media (max-width: 768px) {
  .project-modal {
    width: 90%;
    height: 70%;
    left: 5%;
    top: 15%;
  }
  
  .project-grid {
    grid-template-columns: repeat(auto-fill, minmax(80px, 1fr)); /* Smaller icons on mobile */
  }
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

.tooltip {
  position: fixed;
  background-color: yellow;
  color: black;
  padding: 0.5rem 1rem;
  border: 1px solid #333;
  border-radius: 4px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  pointer-events: none;  /* prevent blocking clicks */
  font-size: 0.9rem;
  z-index: 100;

  white-space: normal;
  max-width: 250px;
  max-height: 150px;
  overflow: auto;
  text-overflow: ellipsis;
}


  </style>
  