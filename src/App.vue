<template>
    <div class="crt-container">
  <div class="screen-frame">
    <div class="screen-inner">
      <audio id="startup-sound" src="@/assets/crt-computer-monitor-startup.wav"></audio>
      <Desktop />
    </div>
  </div>
</div>
</template>

<script setup>
import Desktop from './components/Desktop.vue'

import { onMounted } from 'vue'

onMounted(() => {
  const audio = document.getElementById('startup-sound')
  audio.volume = 0.8
  audio.play()
})


</script>

<style scoped>

.crt-container {
  position: absolute;
  top: 52%;
  left: 57%;
  transform: translate(-50%, -50%);
  width: 80%;
  max-width: 1600px; /* Limits CRT width */
  height: auto;
  display: flex;
  align-items: center;
  justify-content: center;
}


.screen-frame {
  position: relative;
  width: 90vw;  /* Use viewport width for scaling */
  
  max-width: 95%;
  max-height: 85%;
  overflow: hidden;

  transform: translate(-9%, -3%);

  aspect-ratio: 16 / 10;  /* Maintain CRT-like aspect ratio */
  height: auto;
  margin: 2rem auto;
  background: url('@/assets/screen border.png') no-repeat center center;
  background-size: 100% 100%;  /* Stretch to fit within .screen-frame */
  border: 10px solid #333;
  border-radius: 15px;
  box-shadow: 0 10px 50px rgba(0, 0, 0, 0.8), 0 0 20px rgba(0, 255, 0, 0.3) inset;
  display: flex;
  align-items: center;
  justify-content: center;
}

.screen-frame,
.crt-container {
  overflow: visible !important;  /* Allow BSOD to break out of the container */
}

.screen-inner {
  width: 78%;
  height: 76%;
  background: #000;
  border: 1px solid #666;
  overflow: hidden;
  position: relative;
  bottom: 1.6%;
  left: 0.2%;
  aspect-ratio: 4 / 3;  /* Traditional aspect ratio? */
}

@media (max-width: 768px) {
  .screen-frame {
    width: 95vw;  /* Scale down for mobile or small screens */
  }
  .screen-inner {
    width: 90%;
    height: 65%;
  }
}


@keyframes screenFlicker {
  0% {
    opacity: 0;
    filter: brightness(0.1) contrast(3);
  }
  10% {
    opacity: 1;
    filter: brightness(1.5) contrast(1.2);
  }
  20% {
    opacity: 0.4;
  }
  30% {
    opacity: 1;
    filter: brightness(1.2) contrast(1);
  }
  40% {
    opacity: 0.7;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 1;
  }
}

.screen-inner {
  animation: screenFlicker 1.5s ease-out;
  animation-delay: 0.3s;
  opacity: 0;
  animation-fill-mode: forwards;
}

#bsod-screen {
  display: block !important;
  opacity: 1 !important;
}


</style>
