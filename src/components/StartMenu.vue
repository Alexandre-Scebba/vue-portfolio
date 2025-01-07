<template>
  <div class="start-menu" v-if="isOpen">
    <ul>
      <li @click="navigate('Linkedin')"> ℹ️ Linkedin</li> 
      <li @click="navigate('Github')"> 🌐 GitHub</li>
      <li @click="triggerFakeBSOD"> ⚠️ Fake BSOD</li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import bsodImage from '/src/assets/Windows_9X_BSOD.png';

// Define BSOD function
const FakeBSOD = () => {
  console.log(bsodImage);

  const bsod = document.createElement('div');
  bsod.style.position = 'fixed';
  bsod.style.top = 0;
  bsod.style.left = 0;
  bsod.style.width = '100vw';
  bsod.style.height = '100vh';
  bsod.style.background = `url(${bsodImage}) no-repeat center center`;
  bsod.style.backgroundSize = 'cover';
  bsod.style.zIndex = 9999;
  bsod.style.pointerEvents = 'all';
  bsod.id = 'bsod-screen';

  document.body.appendChild(bsod);

  // Remove on keypress or click
  const removeBSOD = () => {
    const screen = document.getElementById('bsod-screen');
    if (screen) {
      document.body.removeChild(screen);
      document.removeEventListener('keydown', removeBSOD);
      document.removeEventListener('click', removeBSOD);
    }
  };

  document.addEventListener('keydown', removeBSOD);
  document.addEventListener('click', removeBSOD);
};

const triggerFakeBSOD = () => {
  FakeBSOD();
};

const isOpen = ref(false)

// Toggle Start Menu Visibility TODO
const toggleMenu = () => {
  isOpen.value = !isOpen.value
}

// Simulate Navigation (Replace with Vue Router if Needed)
const navigate = (section) => {
  console.log(`Navigating to ${section}`)
  isOpen.value = false
}

defineExpose({ toggleMenu })
</script>

<style scoped>
.start-menu {
  position: absolute;
  bottom: 50px;
  left: 20px;
  width: 250px;
  background-color: #333;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  padding: 1rem;
  color: white;
}

.start-menu ul {
  list-style: none;
  padding: 0;
}

.start-menu li {
  padding: 0.75rem 1rem;
  cursor: pointer;
}

.start-menu li:hover {
  background-color: #444;
  border-radius: 5px;
}
</style>
