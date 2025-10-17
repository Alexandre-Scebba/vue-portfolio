<template>
  <div class="xp-window" ref="modalRef" style="top: 10%; left: 50%; position: absolute">
    <div class="xp-title-bar" @mousedown="onMouseDown">
      <span>My CV - Adobe Reader</span>
      <button class="xp-close" @click="$emit('close')">X</button>
    </div>
    <iframe :src="cvPath" class="xp-frame" title="Alex Scebba CV"></iframe>
  </div>
</template>
<script lang="ts" setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  language: { type: String, default: 'EN' },
})

const cvPath = `/Alex Scebba CV 2025-${props.language}.pdf`

const modalRef = ref<HTMLElement | null>(null)
let isDragging = false
let offsetX = 0
let offsetY = 0

const onMouseDown = (e: MouseEvent) => {
  isDragging = true
  const modal = modalRef.value
  if (modal) {
    offsetX = e.clientX - modal.offsetLeft
    offsetY = e.clientY - modal.offsetTop
    document.addEventListener('mousemove', onMouseMove)
    document.addEventListener('mouseup', onMouseUp)
  }
}

const onMouseMove = (e: MouseEvent) => {
  if (isDragging && modalRef.value) {
    modalRef.value.style.left = `${e.clientX - offsetX}px`
    modalRef.value.style.top = `${e.clientY - offsetY}px`
  }
}

const onMouseUp = () => {
  isDragging = false
  document.removeEventListener('mousemove', onMouseMove)
  document.removeEventListener('mouseup', onMouseUp)
}
</script>

<style scoped>
.xp-window {
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translateX(-50%);
  width: 80%;
  max-width: 900px;
  border: 2px solid #000080;
  box-shadow: 5px 5px 0 #808080;
  background-color: #c0c0c0;
  animation: popup 0.4s ease-out;
  overflow: hidden;
  border-radius: 4px;
  font-family: 'Tahoma', sans-serif;
}

.xp-title-bar {
  background-color: #000080;
  color: white;
  padding: 4px 10px;
  display: flex;
  justify-content: space-between;
  font-weight: bold;
  font-size: 14px;
}

.xp-close {
  background-color: #c0c0c0;
  border: 1px solid #808080;
  width: 20px;
  height: 20px;
  font-size: 12px;
  cursor: pointer;
}

.xp-frame {
  width: 100%;
  height: 600px;
  border: none;
}

@keyframes popup {
  from {
    transform: scale(0.7);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
