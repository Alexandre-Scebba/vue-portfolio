<template>
    <div class="desktop">
      <div class="folder-grid">
        <div class="folder" v-for="folder in folders" :key="folder.name" @click="openFolder(folder)">
          <img :src="folder.icon" alt="folder" />
          <p>{{ folder.name }}</p>
        </div>
      </div>

  <!-- Attribution Below the Grid -->
  <div class="attribution">
      <p>
        <a href="https://www.flaticon.com/free-icons/folder" title="folder icons">
          Folder icons created by kmg design - Flaticon
        </a>
      </p>
    </div>
     <!-- Start Menu Component -->
     <StartMenu ref="startMenu" />

<!-- Taskbar / Footer -->
<footer class="taskbar">
  <div class="left-side">
    <button @click="toggleStart">🪟 Start</button>
    <div class="taskbar-divider"></div>
  </div>

  <div class="right-side">
    <div class="language-switch">
      <button @click="toggleLanguage">{{ currentLanguage }}</button>
    </div>
    <div class="taskbar-clock">
      {{ currentTime }}
    </div>
  </div>
</footer>
  </div>

   <!-- Project Modal (Folder View) -->
   <ProjectModal ref="projectModal" :folderName="'Projects'" :projects="projectList" />

</template>
  
  <script setup>

import { ref } from 'vue'
import StartMenu from './StartMenu.vue'
import ProjectModal from './ProjectModal.vue'


const startMenu = ref(null)
const projectModal = ref(null)


  const folders = [
    { name: 'Projects', icon: 'https://cdn-icons-png.flaticon.com/512/716/716784.png'},  /* title="folder icons">Folder icons created by kmg design - Flaticon */
    { name: 'About Me', icon: 'https://cdn-icons-png.flaticon.com/512/4021/4021693.png'},  /* title="folder icons">Folder icons created by kmg design - Flaticon */
    { name: 'Contact', icon: 'https://cdn-icons-png.flaticon.com/512/716/716784.png'},  /* title="folder icons">Folder icons created by kmg design - Flaticon */
  ]

  // Toggle Start Menu
const toggleStart = () => {
  startMenu.value.toggleMenu()
}

// Language Toggle (EN/FR)
const currentLanguage = ref('EN')
const toggleLanguage = () => {
  currentLanguage.value = currentLanguage.value === 'EN' ? 'FR' : 'EN'
}

const currentTime = ref(new Date().toLocaleTimeString())

// Update clock every second
setInterval(() => {
  currentTime.value = new Date().toLocaleTimeString()
}, 1000)

// Sample Project Data
const projectList = [
  {
    name: 'Portfolio Website',
    icon: 'https://cdn-icons-png.flaticon.com/512/1071/1071523.png',
    link: 'https://github.com/yourusername/portfolio'
  },
  {
    name: 'Todo App',
    icon: 'https://cdn-icons-png.flaticon.com/512/1027/1027687.png',
    link: 'https://yourprojecturl.com/todo'
  }
]

// Open the Projects Modal
const openFolder = (folder) => {
  if (folder.name === 'Projects') {
    projectModal.value.openModal()
  }
}


  </script>
  

  <style scoped>
  /* ..............................DESKTOP */
  .desktop {
    height: 100vh;
    display: flex;
    flex-direction: column;

    align-items: stretch;
    justify-content: space-between;
    background: url('https://upload.wikimedia.org/wikipedia/en/2/27/Bliss_%28Windows_XP%29.png') no-repeat center center/cover;
    background-size: cover;  /* Maintain aspect ratio while covering */

  }
  



  /* ..............................FOLDER */
  .folder-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
    gap: 3rem;
    max-width: 1200px;
    padding: 2rem;
    justify-items: start;  /* Align icons to the left */

  }
  
  .folder {
    text-align: center;
    cursor: pointer;
    transition: transform 0.2s ease;
  }
  
  .folder img {
    width: 50px;
    margin-bottom: 0.5rem;
  }
  
  .folder:hover {
    transform: scale(1.15);
  }



  /* ..............................TASKBAR */
  .taskbar {
    width: 100%;
    position: fixed;
    bottom: 0;
    left: 0;
    height: 39.5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(to top, #1c5aa6, #2679c3);
  padding: 0.5rem 1rem;
  box-shadow: 0 -3px 5px rgba(0, 0, 0, 0.5);
  z-index: 10;  /* Keep above other content */
}

.left-side {
  display: flex;
  align-items: center;
}

.right-side {
  display: flex;
  align-items: center;
  margin-left: auto;
}

.taskbar button {
  background: linear-gradient(to top, #3a9d23, #51c33a);
  border: 1px solid #2c761b;
  color: white;
  font-weight: bold;
  padding: 0.5rem 1.5rem;
  cursor: pointer;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 17px;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.4);
  font-size: 1rem;
}

.taskbar button:hover {
  background: linear-gradient(to top, #51c33a, #63e045);
}
.language-switch {
  margin-right: 1rem;
}

.taskbar-divider {
  width: 2px;
  height: 30px;
  background-color: #135aa2;
  margin: 0 10px;
}

.taskbar-clock {
  color: white;
  margin-left: auto;
  margin-right: 1rem;
  font-size: 1rem;
  font-family: 'Courier New', Courier, monospace;
  white-space: nowrap;
}






  /* ..............................ATTRIBUTIONS */

  .attribution {
    position: absolute;
  bottom: 60px;
  right: 20px;

  text-align: center;
  margin-top: 2rem;
  font-size: 0.9rem;
  color: #bbb;
}

.attribution a {
  color: #bbb;
  text-decoration: none;
}

.attribution a:hover {
  color: white;
}



  </style>
  