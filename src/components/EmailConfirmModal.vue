<template>
    <div class="confirm-modal" v-if="isOpen">
      <div class="modal-header">
        <p> {{t.title}} </p>
      </div>
      <div class="modal-body">
        <p> {{t.body  }} </p>
      </div>
      <div class="modal-footer">
        <button @click="confirmEmail"> {{t.yes}} </button>
        <button @click="closeModal"> {{t.no}} </button>
      </div>
    </div>
  </template>
  
  <script setup>
import { ref, defineExpose, defineProps, computed, watch } from 'vue'

const props = defineProps({
  currentLanguage: {
    type: String,
    default: 'EN'
  }
})

  const translations = {
  EN: {
    title: 'Confirm Action',
    body: 'Launch your email client to send a message?',
    yes: 'Yes',
    no: 'No'
  },
  FR: {
    title: 'Confirmer l\'action',
    body: 'Lancer votre client de messagerie pour envoyer un message ?',
    yes: 'Oui',
    no: 'Non'
  }
}
const t = computed(() => translations[props.currentLanguage])
watch(() => props.currentLanguage, (newLang) => {
  t.value = translations[newLang]
})


  const isOpen = ref(false)
  
  const openModal = () => {
    isOpen.value = true
  }
  
  const closeModal = () => {
    isOpen.value = false
  }
  
  const confirmEmail = () => {
    window.location.href = 'mailto:ascebba@gmail.com?subject=Inquiry&body=Hello Alexandre,';
    isOpen.value = false
  }
  
  defineExpose({ openModal })
  </script>
  
  <style scoped>
  .confirm-modal {
    position: fixed;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 350px;
    background-color: #ececec;
    border: 3px solid #1c5aa6;
    border-radius: 8px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
    z-index: 50;
    text-align: center;
    font-family: 'Tahoma', sans-serif;
  }
  
  .modal-header {
    background: #135aa2;
    color: white;
    padding: 0.7rem;
    font-weight: bold;
    font-size: 16px;
  }
  
  .modal-body {
    padding: 1rem;
    font-size: 14px;
    color: black;
  }
  
  .modal-footer {
    display: flex;
    justify-content: space-around;
    padding: 1rem;
  }
  
  .modal-footer button {
    background: #3a9d23;
    color: white;
    padding: 0.5rem 1.5rem;
    border: none;
    border-radius: 6px;
    font-size: 14px;
    cursor: pointer;
  }
  
  .modal-footer button:hover {
    background: #51c33a;
  }
  </style>
  