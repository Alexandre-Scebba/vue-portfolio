<template>
  <div class="start-menu" v-if="isOpen">
    <ul>
      <li @click="navigate('Projects')">📂 Projects</li>
      <li @click="navigate('About')">👤 About Me</li>
      <li @click="navigate('Contact')">📞 Contact</li>
      <li @click="navigate('Settings')">⚙️ Settings</li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

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
