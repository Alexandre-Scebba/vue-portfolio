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
      {{ translations[currentLanguage].Attribution }}
    </a>
  </p>
</div>

     <!-- Start Menu Component -->
     <StartMenu ref="startMenu" />

<!-- Taskbar / Footer INSIDE SCREEN INNER -->
<div class="screen-inner">
<footer class="taskbar">
  <div class="left-side">
    <button @click="toggleStart">{{ translations[currentLanguage].Start }}</button>
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
   <AboutModal :currentLanguage="currentLanguage" ref="aboutModal" />
   <EmailConfirmModal :currentLanguage="currentLanguage" ref="emailModal" />
  </div>
</template>
  
  <script setup>

import { ref } from 'vue'
import StartMenu from './StartMenu.vue'
import ProjectModal from './ProjectModal.vue'
import AboutModal from './AboutModal.vue'
import EmailConfirmModal from './EmailConfirmModal.vue'

import clonifyIcon from '@/assets/project-icons/Clonify logo.png'
import floppyCodeIcon from '@/assets/project-icons/FloppyCode logo.png'
import flashCardIcon from '@/assets/project-icons/FlashCards logo.png'
import codexIcon from '@/assets/project-icons/CODEX logo.png'
import examinaIcon from '@/assets/project-icons/examina logo.png'



const startMenu = ref(null)
const projectModal = ref(null)
const aboutModal = ref(null)
const emailModal = ref(null)

// EN/FR logic 
const currentLanguage = ref('EN')

const translations = {
  EN: {
    Start: 'Start',
    Projects: 'Projects',
    AboutMe: 'About Me',
    Contact: 'Contact',
    CV: 'C.V.',
    Attribution: 'Folder icons created by kmg design - Flaticon'
  },
  FR: {
    Start: 'Démarrer',
    Projects: 'Projets',
    AboutMe: 'À propos de moi',
    Contact: 'Contactez-moi',
    CV: 'C.V.',
    Attribution: 'Icônes de dossier créées par kmg design - Flaticon'
  }
}

// Reactive Folders
const folders = ref([
  { key: 'Projects', icon: 'https://cdn-icons-png.flaticon.com/512/716/716784.png' },
  { key: 'AboutMe', icon: 'https://cdn-icons-png.flaticon.com/512/4021/4021693.png' },
  { key: 'Contact', icon: 'https://cdn-icons-png.flaticon.com/128/2374/2374449.png' },
  { key: 'CV', icon: 'https://cdn-icons-png.flaticon.com/128/14180/14180779.png' }
])

// Function to Update Folder Names Based on Language
const updateFolderNames = () => {
  folders.value = folders.value.map(folder => ({
    ...folder,
    name: translations[currentLanguage.value][folder.key]
  }))
}

// Initialize the folder names
updateFolderNames()

// Toggle Language and Update
const toggleLanguage = () => {
  currentLanguage.value = currentLanguage.value === 'EN' ? 'FR' : 'EN'
  updateFolderNames()
}



  // Toggle Start Menu
const toggleStart = () => {
  startMenu.value.toggleMenu()
}






const currentTime = ref(new Date().toLocaleTimeString())

// Update clock every second
setInterval(() => {
  currentTime.value = new Date().toLocaleTimeString()
}, 1000)

// Project Data
/*
~text-based medical data system
~recreation of sports section of john abbott website
~excuses API
*/
const projectList = [
{
    name: 'Portfolio Website',
    icon: 'https://cdn-icons-png.flaticon.com/128/232/232411.png',
    link: 'https://github.com/yourusername/portfolio'
  },
  {
    name: 'Examina',
    icon:  examinaIcon,
    link: 'https://examina2-cvaehchtdtd4epap.canadaeast-01.azurewebsites.net/'
  },
  {
    name: 'CODEX',
    icon: codexIcon,
    link: 'https://github.com/vvaraksn/SnippetManager'
  },
  {
    name: 'DragonNotes',
    icon: 'https://cdn-icons-png.flaticon.com/128/669/669250.png',
    link: 'https://dragon-notes.vercel.app/'
  },
  {
    name: 'Clonify',
    icon: clonifyIcon,
    link: 'https://github.com/yourusername/clonify'
  },
  {
    name: 'Floppy Code',
    icon: floppyCodeIcon,
    link: 'https://alexandre-scebba.github.io/FloppyCodePage/'
  },
  {
    name: 'Flash Card App',
    icon: flashCardIcon,
    link: 'https://github.com/yourusername/flashcardapp'
  }
]


// Open folder modal logic
const openFolder = (folder) => {
  if (folder.name === 'Projects') {
    projectModal.value.openModal()
  }
  if (folder.name === translations[currentLanguage.value].AboutMe) {
    aboutModal.value.openModal()
  }

  if (folder.name === translations[currentLanguage.value].Contact) {
    emailModal.value.openModal()
}
 if (folder.name === 'C.V.') {
    const cvFile = currentLanguage.value === 'FR' ? 
      '/Alex Scebba CV 2025-FR.pdf' : 
      '/Alex Scebba CV 2025-EN.pdf';
      
    window.open(cvFile, '_blank');
  }
}

  </script>
  

  <style scoped>
  /* ..............................DESKTOP */
  .desktop {
    min-height: clamp(600px, 90vh, 1000px);
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
    gap: clamp(0.5rem, 1.5vw, 2rem);
    max-width: 1200px;
    padding: 2rem;
    justify-items: start;

  }
  
  .folder {
    text-align: center;
    cursor: pointer;
    transition: transform 0.2s ease;
  }
  
  .folder img {
    width: clamp(40px, 5vw, 70px);
    margin-bottom: 0.5rem;
  }
  
  .folder:hover {
    transform: scale(1.15);
  }

  .folder p {
  margin: 0;
  font-size: clamp(0.8rem, 1vw, 1.2rem);
  color: white;
  text-shadow: 1px 1px 2px black, 0 0 1px rgba(0, 0, 0, 0.8); /* black outline */
  font-weight: normal;
  font-family: 'Tahoma', sans-serif;  /* classic XP-style font */
}




  /* ..............................TASKBAR */
  .taskbar {
    width: 100%;
    position: absolute;
    bottom: 0;
    left: 0;
    height: clamp(35px, 5vh, 50px);
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
  padding: clamp(0.3rem, 1vw, 0.5rem) clamp(1rem, 2vw, 1.5rem);
  cursor: pointer;
  border-top-left-radius: clamp(3px, 0.5vw, 6px);
  border-bottom-left-radius: clamp(3px, 0.5vw, 6px);
  border-top-right-radius: clamp(5px, 1vw, 12px);
  border-bottom-right-radius: clamp(8px, 1vw, 17px);
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.4);
  font-size: clamp(0.8rem, 1vw, 1rem);
}


.taskbar button:hover {
  background: linear-gradient(to top, #51c33a, #63e045);
}
.language-switch {
  margin-right: 1rem;
}

.taskbar-divider {
  width: clamp(1px, 0.3vw, 3px);
  height: clamp(20px, 4vh, 40px);
  background-color: #135aa2;
  margin: 0 clamp(5px, 1vw, 15px);
}


.taskbar-clock {
  color: white;
  margin-left: auto;
  margin-right: clamp(0.5rem, 1vw, 1.5rem);
  font-size: clamp(0.8rem, 1vw, 1rem);
  font-family: 'Courier New', Courier, monospace;
  white-space: nowrap;
}







  /* ..............................ATTRIBUTIONS */

  .attribution {
  position: absolute;
  bottom: clamp(40px, 5vh, 80px);
  right: clamp(10px, 2vw, 30px);
  text-align: center;
  margin-top: 1rem;
  font-size: clamp(0.7rem, 1vw, 1rem);
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
  